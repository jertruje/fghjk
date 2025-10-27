# 🎵 musicore - Crie e Personalize suas Playlists

![Badge de Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![Badge de Licença](https://img.shields.io/badge/licen%C3%A7a-MIT-blue)

`musicore` é uma aplicação web interativa que permite aos usuários criar, gerenciar e personalizar playlists de música. O projeto se destaca por oferecer uma experiência de "construtor de site", onde o usuário pode moldar a interface, adicionar seções e compartilhar seus temas personalizados com a comunidade.

<!-- ADICIONE UMA IMAGEM OU GIF DO SEU SITE AQUI -->
<!-- Exemplo:  -->

---

## ✨ Funcionalidades Principais

O projeto é rico em funcionalidades, combinando um player de música com ferramentas de personalização e interação social:

### Player de Música
- **Multi-source:** Toca músicas do **YouTube**, **Spotify** (requer conta Premium) e prévias do **iTunes**.
- **Player Fixo:** Um player de música persistente na parte inferior da tela com controles de reprodução, barra de progresso e informações da faixa.
- **Fila de Reprodução:** Sistema de fila com controles de "próxima" e "anterior".
- **Visual Adaptativo:** A cor do player se adapta dinamicamente à capa do álbum da música que está tocando.
- **Letras e Equalizador:** Visualize as letras das músicas e ajuste o som com um equalizador gráfico.

### 🛠️ Editor de Playlists e Site
- **Criação de Playlists:** Crie e salve múltiplas playlists com nome e descrição.
- **Busca Integrada:** Busque por músicas, artistas e álbuns no Spotify, YouTube e iTunes para adicionar às suas playlists.
- **Construtor de Interface:**
    - **Adicione Abas:** Comece com uma página limpa e adicione seções (abas) como "Editor de Playlists", "Busca de Músicas" e "Comunidade".
    - **Personalização Visual:** Altere a imagem de fundo, cores primárias, fontes, tamanho de botões e arredondamento das bordas.
    - **Modo Escuro:** Alterne entre temas claro e escuro.

### 👥 Comunidade e Compartilhamento
- **Feed da Comunidade:** Uma área onde os usuários podem ver os temas e personalizações criados por outras pessoas.
- **Publicação de Temas:** Usuários logados podem publicar sua própria personalização no feed.
- **Links Personalizados:** Gere um link único e amigável (ex: `.../?theme=seu-nome`) para compartilhar sua versão do site.
- **Autenticação:** Sistema de login com Google, Spotify ou E-mail/Senha para salvar dados na nuvem.
- **Modo Convidado:** Todas as funcionalidades podem ser testadas sem login, com dados salvos localmente no navegador.

---

## 🚀 Tecnologias Utilizadas

- **Frontend:** HTML5, CSS3, JavaScript (ES6+)
- **Autenticação e Banco de Dados:** Firebase (Authentication e Realtime Database)
- **APIs de Música:**
    - **Spotify Web API** & **Web Playback SDK** (para busca e reprodução)
    - **YouTube IFrame Player API** (para reprodução de vídeos)
    - **iTunes Search API** (para busca e prévias)
    - **Lyrics.ovh** (para letras de música)
- **Bibliotecas:**
    - **Font Awesome** (para ícones)
    - **Color Thief** (para extração de cores de imagens)

---

## ⚙️ Como Executar o Projeto

1.  **Clone o repositório (ou baixe os arquivos):**
    ```bash
    git clone https://github.com/seu-usuario/musicore.git
    ```
2.  **Configure o Firebase:**
    - Crie um novo projeto no console do Firebase.
    - Ative os serviços de **Authentication** (com os provedores Google, E-mail/Senha) e **Realtime Database**.
    - Copie as credenciais do seu projeto (apiKey, authDomain, etc.).
    - No arquivo `musica.html`, encontre a variável `firebaseConfig` e substitua os valores pelos da sua conta.

3.  **Configure a API do Spotify:**
    - Crie um aplicativo no Dashboard de Desenvolvedor do Spotify.
    - Adicione a URL onde o site será hospedado (ex: `http://127.0.0.1:5500/musica.html` ou o link do Netlify) nas "Redirect URIs" do seu app Spotify.
    - Copie o **Client ID** e cole na variável `SPOTIFY_CLIENT_ID` no arquivo `musica.html`.

4.  **Abra o arquivo `musica.html`:**
    - Para um funcionamento completo (especialmente o login do Spotify), é recomendado usar um servidor local. Uma extensão como o **Live Server** para o VS Code é uma ótima opção.
    - Ou simplesmente abra o arquivo `musica.html` no seu navegador.

---

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.
