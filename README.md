# Installation
Hent koden
```
git clone https://github.com/roysland/user-switcher.git && cd user-switcher
npm install && npm run build
```

Skru på Cors i programmet du skal jobbe i.
```csharp
Plugins.Add(new CorsFeature());
```
Dette gjøres i Configure.Apphost.cs
## Åpne Chrome/Edge
Åpne extensions siden for [Chrome](chrome://extensions/) eller [Edge](edge://extensions)

1. Skru på Developer mode
2. Load unpacked -> Pek på *dist* mappen i repoet.
3. Enjoy

## Caveats
Antar at du jobber mot Procury og port 4001