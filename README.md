# Packagearkitektur

## 1. Introduktion

Det här projektet är ett demonstrationsprojekt för att lära sig hur kod kan delas upp i fristående packages efter funktion och ansvarsområde. Paketen innehåller avsiktligt lite eller ingen implementationslogik. Fokus ligger på hur de är organiserade, hur de exporterar innehåll och hur de kan användas tillsammans.

Som studerande ska du lära dig att:

- känna igen projektets packageområden och deras roller
- förstå hur ett package är uppbyggt och hur dess publika innehåll exporteras
- avgöra var ny kod hör hemma utifrån om den är grundläggande, central, sidunik eller återanvändbar
- importera ett package via dess namn, till exempel `import { router } from '@project-name/router'`

## 2. Vad är packagearkitektur?

Packagearkitektur innebär att dela upp ett projekt i mindre, namngivna och avgränsade delar. Varje package samlar kod som hör ihop och har ett tydligt ansvarsområde. Ett package kan användas av andra delar av projektet genom sitt publika gränssnitt, i det här projektet vanligtvis dess `index.ts`.

När kod organiseras efter funktionalitet och ansvar blir det lättare att hitta, förstå och återanvända den. Indelningen visar också vilka delar som är generella och vilka som hör till en viss funktion eller sida.

## 3. Projektstruktur

Nedan visas hela strukturen under `packages/`. `package.json` beskriver respektive package, `index.ts` är dess ingång/exportfil och `ui/` respektive `data/` samlar packageinnehåll efter typ.

```text
packages/
├── base/     # Grundläggande UI-byggstenar
│   ├── button/
│   │   ├── package.json
│   │   ├── index.ts
│   │   └── ui/
│   │       ├── index.tsx
│   │       └── index.css
│   ├── inputs/
│   │   ├── checkbox-input/
│   │   │   ├── package.json
│   │   │   ├── index.ts
│   │   │   └── ui/
│   │   │       ├── index.tsx
│   │   │       └── index.css
│   │   └── text-input/
│   │       ├── package.json
│   │       ├── index.ts
│   │       └── ui/
│   │           ├── index.tsx
│   │           └── index.css
│   └── modal/
│       ├── package.json
│       ├── index.ts
│       └── ui/
│           ├── index.tsx
│           └── index.css
├── core/     # Centrala funktioner och tjänster
│   ├── hooks/
│   │   ├── fetch-hook/
│   │   │   ├── package.json
│   │   │   ├── index.ts
│   │   │   └── data/
│   │   │       └── index.tsx
│   │   └── local-storage-hook/
│   │       ├── package.json
│   │       ├── index.ts
│   │       └── data/
│   │           └── index.tsx
│   ├── router/
│   │   ├── package.json
│   │   ├── index.ts
│   │   └── data/
│   │       └── index.tsx
│   ├── stores/
│   │   └── cart-store/
│   │       ├── package.json
│   │       ├── index.ts
│   │       └── data/
│   │           └── index.tsx
│   └── styles/
│       ├── package.json
│       ├── index.ts
│       ├── index.css
│       ├── theme.css
│       ├── typography.css
│       └── variables.css
├── pages/     # Sidor och sidinnehåll
│   ├── home-page/
│   │   ├── package.json
│   │   ├── index.ts
│   │   ├── ui/
│   │   │   ├── index.tsx
│   │   │   └── index.css
│   │   └── children/     # Komponenter knutna till home-page
│   │       ├── home-page-hero/
│   │       │   ├── package.json
│   │       │   ├── index.ts
│   │       │   └── ui/
│   │       │       ├── index.tsx
│   │       │       └── index.css
│   │       └── home-page-newsletter/
│   │           ├── package.json
│   │           ├── index.ts
│   │           └── ui/
│   │               ├── index.tsx
│   │               └── index.css
│   └── product-page/
│       ├── package.json
│       ├── index.ts
│       └── ui/
│           ├── index.tsx
│           └── index.css
└── shared/     # Återanvändbara packages
    ├── contact-form/
    │   ├── package.json
    │   ├── index.ts
    │   └── ui/
    │       ├── index.tsx
    │       └── index.css
    └── product-card/
        ├── package.json
        ├── index.ts
        └── ui/
            ├── index.tsx
            └── index.css
```

## 4. Mapparnas ansvarsområden

- **`base/` – grundläggande UI-byggstenar.** Här finns små, generella gränssnittselement som knappar, inmatningsfält och modal. De utgör en grund som kan användas i större komponenter och på sidor.
- **`core/` – central logik och tjänster.** Här finns funktioner som kan stödja flera delar av applikationen, exempelvis hooks, router, store och gemensamma styles. De är inte knutna till en enskild sida.
- **`pages/` – sidor och deras tillhörande filer.** Varje package motsvarar en sida, exempelvis `home-page` eller `product-page`. Sidans UI och dess eventuella sidunika underkomponenter hör till sidan.
- **`shared/` – återanvändbara komponenter och funktioner.** Här placeras sådant som har ett eget användningsområde och kan återanvändas av flera sidor eller andra delar av projektet, exempelvis ett kontaktformulär eller ett produktkort.

I UI-packages innehåller `ui/index.tsx` komponentens UI och `ui/index.css` dess lokala stilar. Package-roten har en `index.ts` som exporterar UI:t. I packages för hooks, router och store exporterar `index.ts` i stället från `data/index.tsx`. Styles-paketet har CSS-filer för tema, typografi och variabler, samlade via `index.css`.

## 5. Sidunika komponenter och children

Mappen `children/` innehåller packages för komponenter som hör till en viss sida. I projektet ligger `home-page-hero` och `home-page-newsletter` under `pages/home-page/children/`. Deras placering visar att de är delar av just `home-page`, snarare än fristående, allmänna UI-element.

En komponent placeras under en specifik sida när dess syfte och användning är knutna till den sidans innehåll eller uppbyggnad. Det håller sidans delar nära sidan och gör sambandet tydligt.

Om en komponent eller funktion är generell och tänkt att kunna användas oberoende av en viss sida, hör den i stället hemma i `shared/`. Exempel på sådana packages i projektet är `contact-form` och `product-card`.

## 6. Var ska nya filer placeras?

Utgå från funktionens ansvar och vem som använder den:

| Typ av kod | Placering | Exempel |
|---|---|---|
| Grundläggande UI-komponent | `base/<komponent>/ui/` | En generell knapp eller ett inmatningsfält |
| Hook | `core/hooks/<hook-namn>/data/` | En generell hook, enligt mönstret för `fetch-hook` |
| Store | `core/stores/<store-namn>/data/` | En gemensam applikationsstore, enligt `cart-store` |
| Router eller annan central tjänst | `core/<tjänst>/data/` | Router-paketet ligger i `core/router/` |
| Sidspecifik komponent | `pages/<sidnamn>/children/<komponent>/ui/` | En hero-del som bara hör till en viss sida |
| Återanvändbar komponent eller funktion | `shared/<paketnamn>/` | En komponent som kan användas av flera sidor |
| Paketets publika export | Paketets `index.ts` | Exportera det innehåll som andra packages ska kunna importera |
| Package-metadata | Paketets `package.json` | Ange package-namn och `main`-fil |

Exempelvis hör en hook som bara hanterar en viss sidas särskilda beteende nära den sidan. En generell hook, som kan användas på flera ställen, hör till `core/hooks/`. Samma princip gäller komponenter: sidunik funktionalitet placeras under den sidan, medan återanvändbar funktionalitet placeras i `shared/`. Typer placeras nära det package som äger och använder dem; gemensamma typer hör hemma nära den gemensamma funktionalitet de beskriver.

I det här demonstrationsprojektet finns inga separata exempel på services eller typer. Tabellen beskriver därför hur placering kan avgöras utifrån den befintliga packageindelningen, inte ytterligare mappar som redan finns.

Packages importeras med sitt namn, inte genom att ange den interna filsökvägen. Exempel:

```ts
import { router } from '@project-name/router'
```

Package-namnet kommer från dess `package.json`, och `index.ts` fungerar som paketets publika ingång.

## 7. Sammanfattning

- Dela upp kod efter funktion och tydligt ansvarsområde.
- Lägg centrala funktioner i `core/`, grundläggande UI-byggstenar i `base/` och sidor i `pages/`.
- Håll sidunika delar under den sida de tillhör, till exempel i `children/`.
- Placera sådant som är generellt och återanvändbart i `shared/`.
- Använd package-roten och dess `index.ts` som den publika exportvägen.
- Importera packages via deras namn, exempelvis `@project-name/router`.
