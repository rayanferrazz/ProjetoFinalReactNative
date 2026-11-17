# LoL Companion — Projeto Final React Native

![React Native](https://img.shields.io/badge/React%20Native-0.72.6-61DAFB?logo=react&logoColor=black)
![Expo](https://img.shields.io/badge/Expo-49-000020?logo=expo&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![Riot API](https://img.shields.io/badge/Riot%20API-League%20of%20Legends-D32936)

Aplicativo mobile inspirado no universo de **League of Legends**, desenvolvido como trabalho final da disciplina de **React Native** da Residência de Software do **Serratec**.

O app combina autenticação local, catálogo de campeões, detalhes de habilidades, rotação semanal gratuita e busca de invocadores com **rank** e **maestria**, consumindo a **Riot Games API** e a **Data Dragon**.

---

## Sumário

- [Funcionalidades](#funcionalidades)
- [Telas](#telas)
- [Tecnologias](#tecnologias)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Pré-requisitos](#pré-requisitos)
- [Como executar](#como-executar)
- [Configuração do JSON Server](#configuração-do-json-server)
- [Configuração da Riot API](#configuração-da-riot-api)
- [Endpoints externos utilizados](#endpoints-externos-utilizados)
- [Autenticação local](#autenticação-local)
- [Problemas conhecidos](#problemas-conhecidos)
- [Melhorias sugeridas](#melhorias-sugeridas)

---

## Funcionalidades

- **Login e cadastro** com validação de e-mail e senha forte;
- Persistência local de usuários com **AsyncStorage**;
- **Home** com:
  - Lista de campeões carregada da Data Dragon;
  - Busca de campeões por nome;
  - Ícones por classe (Assassino, Mago, Lutador, Tanque, Suporte, Atirador);
  - Modal com história, classe e habilidades (Q/W/E/R);
  - Modal de detalhes de cada habilidade;
  - Rotação semanal de campeões gratuitos.
- **Busca de jogador** com:
  - Nome do invocador;
  - Ícone e nível;
  - Rank (tier/divisão), vitórias, derrotas e win rate;
  - Campeão com maior maestria;
  - Imagem de fundo do campeão.
- **Navegação** com Stack + Bottom Tabs;
- **Fontes customizadas** (Beaufort for LOL);
- **Splash screen** e tema escuro.

## Telas

| Tela | Arquivo | Descrição |
| --- | --- | --- |
| Login | `src/screens/Login/index.tsx` | Autenticação local com animação e validações |
| Cadastro | `src/screens/Cadastro/index.tsx` | Criação de conta no AsyncStorage e no JSON Server |
| Home | `src/screens/Home/index.tsx` | Campeões, busca, habilidades e rotação gratuita |
| Buscar Jogador | `src/screens/SearchPlayer/index.tsx` | Perfil, rank e maestria via Riot API |

## Tecnologias

| Camada | Tecnologia |
| --- | --- |
| Mobile | React Native 0.72.6 + Expo 49 |
| Linguagem | TypeScript 5 |
| Navegação | React Navigation (Stack + Bottom Tabs) |
| HTTP | Axios |
| Persistência local | `@react-native-async-storage/async-storage` |
| UI | `expo-linear-gradient`, `react-native-modal`, `expo-status-bar` |
| Ícones | `@expo/vector-icons` |
| Fontes | `expo-font` |
| Mock de usuários | `json-server` |
| APIs externas | Riot Games API + Data Dragon |

## Estrutura do projeto

```text
.
├── App.tsx                        # Carrega fontes e inicia as rotas
├── app.json                       # Configuração do Expo
├── babel.config.js
├── db.json                        # Base do json-server (usuários)
├── package.json
├── react-native.config.js
├── src/
│   ├── @types/tipos.d.ts
│   ├── assets/                    # Imagens, ícones de rank, ícones de classe
│   ├── components/
│   │   ├── ButtonPesquisa/
│   │   ├── ChampionModal/
│   │   ├── ChampionsList/
│   │   ├── HabilidadeModal/
│   │   ├── Header/
│   │   ├── InputPesquisa/
│   │   ├── LoginContainer/
│   │   ├── LoginFooter/
│   │   ├── LoginHeader/
│   │   ├── PesquisaCampeoes/
│   │   ├── PlayerCard/
│   │   └── RotacaoGratis/
│   ├── contexts/auth.tsx          # Contexto do usuário logado
│   ├── Routes/
│   │   ├── index.tsx
│   │   ├── BottomTabNav/index.tsx
│   │   └── Stack/stack.routes.tsx
│   ├── screens/
│   │   ├── Cadastro/
│   │   ├── Home/
│   │   ├── Login/
│   │   └── SearchPlayer/
│   ├── service/
│   │   ├── apiLol/index.ts        # Riot API + Data Dragon
│   │   └── jsonServer/index.ts    # URL do json-server
│   └── utils/CarregaFonte/        # Carregamento das fontes
└── tsconfig.json
```

## Pré-requisitos

- **Node.js 18+** e npm;
- **Expo Go** no celular ou emulador Android/iOS;
- **JSON Server** (`npm install -g json-server`) para o cadastro de usuários;
- **Chave de desenvolvedor da Riot Games** válida.

## Como executar

### 1. Instalar as dependências

```bash
npm install
```

Se houver conflito de peer dependencies:

```bash
npm install --legacy-peer-deps
```

### 2. Adicionar as fontes

O `App.tsx` só renderiza depois que as fontes são carregadas em `src/utils/CarregaFonte/index.tsx`:

```text
assets/fonts/BeaufortforLOL-Bold.ttf
assets/fonts/BeaufortforLOL-Regular.ttf
```

> ⚠️ A pasta `assets/fonts` **não está incluída no repositório**. Crie-a e adicione os arquivos `.ttf` (disponíveis no site oficial de desenvolvimento da Riot/League of Legends ou em repositórios de recursos do jogo). Sem as fontes, o `App` permanece retornando `null`.

### 3. Configurar o JSON Server

Edite `src/service/jsonServer/index.ts` e informe o IP da sua máquina:

```ts
const ip = "192.168.0.10"; // troque pelo IP local
```

Depois, na raiz do projeto:

```bash
json-server --watch db.json --host 192.168.0.10 --port 3000
```

> O celular e o computador precisam estar na **mesma rede**. No emulador Android, `10.0.2.2` costuma funcionar no lugar do IP local.

### 4. Iniciar o app

```bash
npx expo start
```

- `a` → emulador Android;
- `i` → simulador iOS (macOS);
- QR Code → Expo Go no celular.

## Configuração da Riot API

A chave está **hardcoded** em `src/service/apiLol/index.ts`:

```ts
const API_KEY = "RGAPI-...";
```

> 🔐 **Importante**: chaves `RGAPI-` expiram em **24 horas** e não devem ser commitadas. Substitua pela sua chave e, em um projeto real, use variáveis de ambiente/backend proxy. A chave presente no repositório provavelmente **já expirou**.

A versão da Data Dragon usada é **13.22.1** (fixa em várias URLs). Para atualizar, altere o trecho `cdn/13.22.1/data/pt_BR/...` e as URLs de imagens nos componentes.

## Endpoints externos utilizados

### Data Dragon (sem chave)

| Função | URL |
| --- | --- |
| Lista de campeões | `https://ddragon.leagueoflegends.com/cdn/13.22.1/data/pt_BR/champion.json` |
| Detalhes do campeão | `https://ddragon.leagueoflegends.com/cdn/13.22.1/data/pt_BR/champion/{campeao}.json` |
| Imagem do campeão | `https://ddragon.leagueoflegends.com/cdn/13.22.1/img/champion/{campeao}.png` |
| Splash do campeão | `https://ddragon.leagueoflegends.com/cdn/img/champion/loading/{campeao}_0.jpg` |
| Ícone de habilidade | `https://ddragon.leagueoflegends.com/cdn/13.22.1/img/spell/{id}.png` |
| Ícone de perfil | `https://ddragon.leagueoflegends.com/cdn/13.22.1/img/profileicon/{id}.png` |

### Riot API (requer chave)

| Função | Endpoint |
| --- | --- |
| Invocador por nome | `/lol/summoner/v4/summoners/by-name/{nome}` |
| Ranks | `/lol/league/v4/entries/by-summoner/{id}` |
| Maestria | `/lol/champion-mastery/v4/champion-masteries/by-puuid/{puuid}/top` |
| Rotação semanal | `/lol/platform/v3/champion-rotations` |

> ⚠️ A Riot substituiu o endpoint `by-name` pelo conceito de **Riot ID** (`gameName#tagLine`). Dependendo da versão da API, a busca por nome pode retornar `404`. Será necessário migrar para `/lol/summoner/v4/summoners/by-riot-id/{gameName}/{tagLine}`.

## Autenticação local

O fluxo de login **não consulta o `db.json`**:

1. O cadastro salva o usuário no **AsyncStorage** com a chave `user_{email}`;
2. Em paralelo, envia o usuário para o JSON Server (`POST /users`);
3. O login lê apenas o AsyncStorage e compara e-mail/senha.

Consequências:

- Os usuários pré-cadastrados em `db.json` **não conseguem logar** até serem cadastrados pelo app;
- Limpar os dados do app apaga os usuários locais;
- Senhas ficam salvas em texto puro no AsyncStorage;
- O JSON Server é opcional para o login, mas obrigatório para o `POST` do cadastro (se estiver offline, o cadastro local já terá ocorrido e o erro do `post` ficará sem tratamento).

## Problemas conhecidos

- **Fontes ausentes** — `assets/fonts` não existe no repositório e o app fica em branco se o carregamento falhar.
- **Ícones do app ausentes** — `app.json` referencia `./assets/icon.png`, `./assets/adaptive-icon.png` e `./assets/favicon.png`, que não estão no repositório.
- **Chave da Riot exposta/expirada** — ver seção acima.
- **`by-name` possivelmente obsoleto** — migrar para Riot ID.
- **Senhas em texto puro** no AsyncStorage e no `db.json`.
- **Sem tratamento de erro de rede** no cadastro (o `post` não tem `try/catch`).
- **`db.json` com usuários de exemplo** — não usar dados reais.
- **Versão fixa da Data Dragon** — campeões novos não aparecem até atualizar.
- **`@expo/vector-icons`** é usado, mas não está listado explicitamente no `package.json`; em alguns ambientes pode ser necessário instalá-lo.
- **`react-native-gesture-handler` ausente** — o pacote `@react-navigation/drawer` está instalado, mas não é usado; se for usar Drawer, instale o gesture handler.

## Melhorias sugeridas

- Remover segredos do código e usar `app.config.ts` + `expo-constants`/variáveis de ambiente;
- Migrar a autenticação para um backend real com hash de senha e JWT;
- Atualizar a integração com a Riot API para Riot ID;
- Adicionar estados de carregamento/erro e *pull to refresh*;
- Adicionar cache das respostas (React Query/SWR);
- Incluir as fontes e ícones no repositório ou baixá-los em tempo de execução;
- Escrever testes com Jest + React Native Testing Library;
- Adicionar tipagem forte nas respostas da API (interfaces);
- Melhorar acessibilidade e internacionalização.

## Contexto acadêmico

Trabalho final da disciplina de **React Native** — Residência de Software **Serratec**.

## Autor

Desenvolvido por **Rayan Ferraz** e equipe.

- GitHub: [@rayanferrazz](https://github.com/rayanferrazz)
