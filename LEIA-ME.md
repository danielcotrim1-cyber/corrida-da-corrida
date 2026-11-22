# Corrida do Corrida das Blogueiras: versão para o GitHub Pages

Esta pasta tem o mesmo site do link do Claude. A diferença é a urna: aqui os votos ficam no **Firebase Realtime Database**, então qualquer pessoa vota sem login e sem limite.

## Arquivos

- `index.html`: o site inteiro (visual, dados das participantes, semanas e votação).
- `database.rules.json`: as regras de segurança do banco. Elas só aceitam votos que somam +1, nas perguntas que existem.
- `assets/`: texturas (damasco e papel), botões (ouro e vinho), ornamento e moldura, gerados no Magnific com Nano Banana 2.1. Suba a pasta junto com o `index.html`.
- `LEIA-ME.md`: este passo a passo.

## 1. Criar o banco no Firebase (uns 10 minutos)

1. Entre em console.firebase.google.com com sua conta Google e clique em **Criar projeto**. O Google Analytics pode ficar desligado.
2. No menu do projeto, vá em **Build > Realtime Database > Criar banco de dados**.
   - Local: `us-central1` (é o padrão; serve bem).
   - Modo: **bloqueado** (as regras do passo 3 liberam só o que precisa).
3. Na aba **Regras** do banco, apague o que estiver lá, cole o conteúdo de `database.rules.json` e clique em **Publicar**.
4. Volte para a página inicial do projeto e clique no ícone **Web `</>`** para registrar um app.
   - Dê um nome qualquer e não marque a opção de Hosting.
   - O Firebase mostra um bloco `firebaseConfig = {...}`. Copie os valores de `apiKey`, `authDomain`, `databaseURL`, `projectId` e `appId`.
   - Se o `databaseURL` não aparecer, pegue o endereço no topo da página do Realtime Database (algo como `https://seu-projeto-default-rtdb.firebaseio.com`).

## 2. Colar a configuração no site

Abra `index.html`, procure por `FIREBASE_CONFIG` e preencha:

```js
const FIREBASE_CONFIG={apiKey:"AIza...",authDomain:"seu-projeto.firebaseapp.com",databaseURL:"https://seu-projeto-default-rtdb.firebaseio.com",projectId:"seu-projeto",appId:"1:123:web:abc"};
```

Esses valores podem ficar públicos no site. Quem protege o banco são as regras do passo 3.

## 3. Publicar no GitHub Pages

1. Crie um repositório público no GitHub (exemplo: `corrida-do-corrida`).
2. Suba os arquivos desta pasta (`index.html` na raiz).
3. Vá em **Settings > Pages**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`, e salve.
4. Em um ou dois minutos o site fica no ar em `https://SEU-USUARIO.github.io/corrida-do-corrida/`.

## Toda semana

Atualize o `index.html` (ou me peça para atualizar) com:

- `WEEKS`: um bloco novo para o episódio. Ele tem o `id` (s5, s6…), o título da prova, a data em que a votação abre e fecha, o `cast` de quem participou, o `real` com o resultado do programa e o `looks` com as fotos dos looks.
- `CAST`: o status de quem saiu (`st:"out"` e `note`).
- `PHOTOS`: as fotos das participantes.

Os votos de cada semana ficam guardados separados no banco (`votes/s4`, `votes/s5`…). O voto da Coroa fica em `votes/coroa` e continua acumulando até a final.

## Limites do plano grátis (Spark)

- **Armazenamento:** 1 GB. Os votos ocupam quase nada, porque cada participante é só um número.
- **Transferência:** 10 GB por mês. Cada pessoa com o site aberto recebe o placar ao vivo, então a transferência é o que mais cresce se o site bombar.
- **Conexões simultâneas:** até 100 pessoas com o site aberto ao mesmo tempo.

Se passar desses números, dá para mudar para o plano Blaze, que cobra por uso.

## Sobre votos repetidos

Do jeito que você pediu, o voto é livre e ilimitado. As regras impedem que alguém coloque um número falso no placar, porque só aceitam +1 por vez. Mas não impedem que alguém crie um robô que clica mil vezes. Se isso virar problema, dá para ligar o **App Check** do Firebase depois.
