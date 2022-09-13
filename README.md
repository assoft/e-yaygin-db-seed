# E-Yaygın Program Listesi

E-yaygın üzerinde yayınlanan bütün kurs programlarını içerir.

[Prisma](!https://www.prisma.io/docs/guides/database/seed-database) için dahili besleme kodu sağlar.

- Kaynak URL: https://e-yaygin.meb.gov.tr/pagePrograms.aspx
- Bağımlılıklar
  - slugify (https://www.npmjs.com/package/slugify)

`schema.prisma`

```ts
model Course {
  id                String   @id @default(cuid())
  name              String
  slug              String   @unique
  minAge            Int      @db.UnsignedInt
  minEducationLevel Int      @db.UnsignedInt
  isActive          Boolean  @default(false)
  categoryId        String
  category          Category @relation(fields: [categoryId], references: [id])

  @@index([categoryId], map: "courses_categoryId_fkey")
  @@map("courses")
}

model Category {
  id       String   @id @default(cuid())
  name     String   @unique
  isActive Boolean  @default(false)
  courses  Course[]

  @@map("categories")
}
```

`prisma/seed.ts`

```ts
import { generatedData } from "./categories.ts";

generatedData.map(async (category) => {
  await prisma.category.create({
    data: category,
  });
});
```
