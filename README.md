# KETER
Identity Server

### Stack Tecnologico :

1. Runtime: Node.js (v20+) con TypeScript.
2. Framework: Fastify (più veloce di Express, ideale per microservizi).
3. Database: PostgreSQL (Dati utenti).
4. Cache/Sessioni: Redis (per revoca token e rate limiting).
5. ORM: Drizzle ORM (estremamente leggero e "type-safe").
6. Hashing: Argon2 (più sicuro e veloce di Bcrypt).
   
### Il Flusso di Autenticazione (Logica del Microservizio)
Il servizio esporrà principalmente tre endpoint:

1. POST /register: Riceve email/password, valida con Zod, effettua l'hash con Argon2 e salva su Postgres.
2. POST /login: Verifica le credenziali e restituisce due token:
   2.1 Access Token (JWT): Breve durata (es. 15 min), salvato in memoria dal client.
   2.2 Refresh Token: Lunga durata (es. 7 giorni), salvato in un cookie httpOnly o nel DB.
3. GET /verify: Usato dagli altri microservizi (o dal Gateway) per validare il JWT.
