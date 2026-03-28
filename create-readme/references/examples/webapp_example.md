# Example: Web Application README

This is an example of a well-structured web application README.

```markdown
# T3 Stack Example

A reference implementation of the T3 Stack - Next.js, tRPC, Prisma, Tailwind CSS, and NextAuth.

[![Build Status][build-badge]][build-url]
[![License][license-badge]][license-url]

> The best way to create a type-safe full-stack React application.

## Features

- 🚀 **Serverless** - Deploys to Vercel
- 🔒 **Authentication** - NextAuth.js with GitHub provider
- 📊 **Database** - Prisma with PostgreSQL
- 🎨 **Styling** - Tailwind CSS
- ⭐ **Type Safety** - TypeScript + tRPC
- 📱 **Mobile** - Responsive design

## Quick Start

```bash
# Clone the repository
git clone https://github.com/example/t3-app.git
cd t3-app

# Install dependencies
npm install

# Set up database
npx prisma db push

# Start development
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Getting Started

### Prerequisites

- Node.js 18+
- npm 9+
- PostgreSQL (local or hosted)

### Installation

1. **Clone the repo**
   ```bash
   git clone https://github.com/example/t3-app.git
   cd t3-app
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment**
   ```bash
   cp .env.example .env
   ```

4. **Set up database**
   ```bash
   # Push schema to database
   npx prisma db push

   # Seed data (optional)
   npx prisma db seed
   ```

5. **Start development**
   ```bash
   npm run dev
   ```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `NEXTAUTH_SECRET` | NextAuth secret | Yes |
| `NEXTAUTH_URL` | NextAuth URL | Yes |
| `GITHUB_CLIENT_ID` | GitHub OAuth client ID | Yes |
| `GITHUB_CLIENT_SECRET` | GitHub OAuth secret | Yes |

> [!NOTE]
> Get GitHub credentials at [GitHub OAuth Apps](https://github.com/settings/developers)

## Project Structure

```
src/
├── app/                    # Next.js App Router
│   ├── api/               # API routes
│   │   ├── auth/          # NextAuth endpoints
│   │   └── trpc/           # tRPC endpoints
│   ├── (auth)/            # Auth pages
│   │   ├── login/
│   │   └── register/
│   ├── (dashboard)/       # Protected pages
│   │   └── dashboard/
│   ├── _components/       # App components
│   ├── _trpc/             # tRPC client
│   └── page.tsx           # Home page
├── server/
│   ├── api/
│   │   ├── routers/       # tRPC routers
│   │   └── root.ts       # Root router
│   ├── auth/              # NextAuth config
│   └── db.ts             # Prisma client
├── styles/
│   └── globals.css        # Global styles
└── utils/
    └── trpc.ts            # tRPC utilities
```

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start dev server |
| `npm run build` | Build for production |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm run db:push` | Push Prisma schema |
| `npm run db:studio` | Open Prisma Studio |
| `npm run postinstall` | Generate types |

## Tech Stack

| Technology | Purpose |
|------------|---------|
| [Next.js 14](https://nextjs.org) | React framework |
| [tRPC](https://trpc.io) | Type-safe API |
| [Prisma](https://prisma.io) | Database ORM |
| [Tailwind CSS](https://tailwindcss.com) | Styling |
| [NextAuth.js](https://next-auth.js.org) | Authentication |
| [TypeScript](https://typescriptlang.org) | Type safety |

## API Usage

### Fetching Data

```typescript
import { api } from "@/trpc/react";

function UserList() {
  const { data: users, isLoading } = api.user.list.useQuery();

  if (isLoading) return <div>Loading...</div>;

  return (
    <ul>
      {users?.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### Mutations

```typescript
import { api } from "@/trpc/react";

function CreateUser() {
  const createUser = api.user.create.useMutation({
    onSuccess: () => {
      // Invalidate and refetch
      void utils.user.list.invalidate();
    },
  });

  const handleCreate = () => {
    createUser.mutate({
      name: "New User",
      email: "user@example.com",
    });
  };

  return <button onClick={handleCreate}>Create User</button>;
}
```

## Deployment

### Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel
```

### Docker

```bash
# Build
docker build -t t3-app .

# Run
docker run -p 3000:3000 t3-app
```

### Manual

```bash
# Build
npm run build

# Start
npm run start
```

## Database Schema

```prisma
model User {
  id            String    @id @default(cuid())
  name          String?
  email         String?   @unique
  emailVerified DateTime?
  image         String?
  accounts      Account[]
  sessions      Session[]
}

model Account {
  id                String  @id @default(cuid())
  userId            String
  type              String
  provider          String
  providerAccountId String
  refresh_token     String?
  access_token      String?
  expires_at        Int?
  token_type        String?
  scope             String?
  id_token          String?
  session_state     String?

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([provider, providerAccountId])
}
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

1. Fork the repository
2. Create your branch
3. Make changes
4. Run tests
5. Submit PR

## License

MIT - see [LICENSE](LICENSE)

---

[build-badge]: https://github.com/example/t3-app/actions/workflows/main.yml/badge.svg
[license-badge]: https://img.shields.io/npm/l/next
```

## Key Takeaways

1. **Features list** - What's included
2. **Quick Start** - 5-step setup
3. **Environment variables** - Clear table
4. **Project structure** - Tree view
5. **Scripts** - npm commands table
6. **Tech stack** - Table with links
7. **Code examples** - Real usage
8. **Deployment** - Multiple options
9. **Schema** - Database diagram
