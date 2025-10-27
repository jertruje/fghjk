<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>musicore - Crie e personalize suas playlists</title>
    <style>
        :root {
            --primary-color: #6a11cb;
            --secondary-color: #2575fc;
            --accent-color: #ff6b6b;
            --dark-color: #2d3436;
            --light-color: #f7f9fc;
            --text-color: #333;
            --border-radius: 12px;
            --box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }
        .feed-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 1.5rem;
        }
        .feed-post-card {
            background: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            overflow: hidden;
            transition: var(--transition);
        }
        body.dark-mode .feed-post-card {
            background: #3b444b;
        }
        .feed-post-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0,0,0,0.15);
        }
        .feed-post-content { padding: 1rem; }
        .feed-post-title { font-size: 1.1rem; font-weight: 600; margin-bottom: 0.25rem; }
        .feed-post-author { font-size: 0.9em; color: #777; margin-bottom: 1rem; }
        body.dark-mode .feed-post-author { color: #ccc; }


        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, var(--light-color) 0%, #e0eafc 100%);
            background-size: cover;
            color: var(--text-color);
            min-height: 100vh;
            line-height: 1.6;
        }
        body.dark-mode {
            --light-color: #2d3436;
            --text-color: #f7f9fc;
            background: linear-gradient(135deg, #2c3e50 0%, #000000 100%);
        }

        header {
            background: linear-gradient(to right, var(--primary-color), var(--secondary-color));
            color: white;
            padding: 1.5rem 2rem;
            box-shadow: var(--box-shadow);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            max-width: 1200px;
            margin: 0 auto;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 1.8rem;
            font-weight: 700;
        }

        .logo i {
            font-size: 2rem;
        } 

        nav ul {
            display: flex;
            list-style: none;
            gap: 2rem;
        }

        nav a {
            color: white;
            text-decoration: none;
            font-weight: 500;
            padding: 0.5rem 1rem;
            border-radius: var(--border-radius);
            transition: var(--transition);
        }

        nav a:hover, nav a.active {
            background-color: rgba(255, 255, 255, 0.2);
        }

        .user-profile {
            color: white;
            display: flex;
            align-items: center;
            gap: 1rem;
        }

        main {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 1.5rem;
            padding-bottom: 120px; /* Espaço para o player fixo */
            min-height: calc(100vh - 180px); /* Altura mínima considerando o player */
            box-sizing: border-box; /* Garante que padding não adicione largura */
        }

        .section-title {
            margin-bottom: 1.5rem;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.5rem;
        }

        .section-title i {
            color: var(--primary-color);
        }

        .tab-container {
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            overflow: hidden;
            margin-bottom: 2rem;
        }
        body.dark-mode .tab-container {
            background-color: #3b444b; /* Um pouco mais claro para contraste */
            box-shadow: var(--box-shadow);
            overflow: hidden;
            margin-bottom: 2rem;
        }

        .tabs {
            display: flex;
            background-color: #f1f3f9;
            border-bottom: 1px solid #e0e0e0; /* Mantém a borda para separação */
        }
        body.dark-mode .tabs {
            background-color: #2d3436; /* Cor de fundo para abas no modo escuro */
            border-bottom: 1px solid #444;
        }

        .tab {
            padding: 1rem 1.5rem;
            cursor: pointer;
            transition: var(--transition);
            font-weight: 600; /* Aumenta o peso da fonte para destaque */
            flex-grow: 1; /* Permite que as abas se estiquem */
            text-align: center;
        }
 
        .tab.active {
            background-color: white;
            border-bottom: 3px solid var(--primary-color);
        }
        body.dark-mode .tab.active {
            background-color: #3b444b;
        }

        .tab-content {
            padding: 2rem;
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .playlist-manager {
            display: grid;
            grid-template-columns: 300px 1fr; /* Aumenta um pouco a sidebar */
            gap: 2rem;
        }

        .playlist-sidebar {
            border-right: 1px solid #eee;
            padding-right: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .playlist-list-item {
            padding: 0.75rem 1rem;
            cursor: pointer;
            border-radius: 8px;
            transition: var(--transition);
            font-weight: 500;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .playlist-list-item:hover, .playlist-list-item.active {
            background-color: #e9ecef;
            color: var(--primary-color);
        }

        .album-creator, .playlist-creator {
            display: grid;
            grid-template-columns: 1fr 2.5fr; /* Dá mais espaço para a lista de faixas */
            gap: 2rem;
        }

        .album-cover {
            background-color: #f5f5f5;
            border-radius: var(--border-radius);
            height: 300px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 1rem;
            padding: 1rem;
            text-align: center;
            box-shadow: var(--box-shadow);
            position: relative;
            overflow: hidden;
        }

        .album-cover img {
            max-width: 100%;
            max-height: 200px;
            border-radius: 8px;
        }

        .album-details {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        label {
            font-weight: 500;
        }

        input, textarea, select {
            padding: 0.75rem;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            font-size: 1rem;
            transition: var(--transition);
        }

        input:focus, textarea:focus, select:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 2px rgba(106, 17, 203, 0.2);
        }

        .btn {
            padding: 0.75rem 1.5rem;
            background: linear-gradient(to right, var(--primary-color), var(--secondary-color));
            color: white;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-weight: 500;
            transition: var(--transition);
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
        }

        .btn-outline {
            background: transparent;
            border: 1px solid var(--primary-color);
            color: var(--primary-color);
        }

        .track-list {
            margin-top: 1.5rem;
        }

        .track-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0.75rem;
            border-bottom: 1px solid #eee;
            transition: var(--transition);
        }

        .track-item:hover {
            background-color: #f9f9f9;
        }

        .track-info {
            display: flex;
            align-items: center;
            gap: 1rem;
        }

        .track-number {
            color: #777;
            width: 30px;
            text-align: center;
        }

        .track-actions {
            display: flex;
            gap: 0.5rem;
        }

        .icon-btn {
            background: none;
            border: none;
            cursor: pointer;
            color: #777;
            transition: var(--transition);
            padding: 5px;
            border-radius: 50%;
        }

        .icon-btn:hover {
            background-color: #f0f0f0;
            color: var(--primary-color);
        }

        .search-section {
            margin-bottom: 2rem;
        }

        .search-box {
            display: flex;
            gap: 1rem;
            margin-bottom: 1.5rem;
        }

        .search-box input {
            flex: 1;
        }

        .search-results {
            max-height: 400px;
            overflow-y: auto;
            border: 1px solid #eee;
            border-radius: var(--border-radius);
        }

        .search-result-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem;
            border-bottom: 1px solid #eee;
            transition: var(--transition);
        }

        .search-result-item:hover {
            background-color: #f9f9f9;
        }

        .player {
            position: fixed;
            left: 0;
            right: 0;
            bottom: 0;
            z-index: 99999;
            width: 100%;
            padding: 15px 20px;
            background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
            transition: transform 0.3s ease, background 1s ease, height 0.3s ease;
            height: 130px; /* Altura padrão do player recolhido */
        }

        .player-content {
            max-width: 900px;
            margin: 0 auto;
            color: white;
        }

        /* Estilos para o player expandido */
        .player.expanded {
            height: 100vh; /* Ocupa a tela inteira */
            padding: 2rem;
        }
        .player.expanded .player-content {
            flex-direction: column;
            height: 100%;
            justify-content: center;
        }
        .player.expanded .player-info {
            flex-direction: column;
            text-align: center;
            gap: 1.5rem;
        }
        .player.expanded .player-info img {
            width: 250px;
            height: 250px;
        }
        
        .player.hidden {
            transform: translateY(100%);
        }

        #spotify-player {
            width: 100%;
            max-width: 900px;
            margin: 0 auto;
            display: block;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }

        #spotify-web-player {
            width: 100%;
            max-width: 900px;
            margin: 0 auto;
        }

        .player-info {
            display: flex;
            align-items: center;
            gap: 1rem;
            margin-bottom: 1rem;
        }

        .player-info img {
            width: 60px;
            height: 60px;
            border-radius: 8px;
            object-fit: cover;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        }

        .track-details h4 {
            margin-bottom: 5px;
            color: white;
            font-size: 1.1rem;
            font-weight: 600;
            transition: color 0.5s ease;
        }

        .track-details p {
            color: rgba(255, 255, 255, 0.8);
            font-size: 0.9rem;
        }

        .player-ui-controls {
            position: absolute;
            top: 10px;
            right: 20px;
            display: flex;
            gap: 0.5rem;
        }

        .player-ui-controls .icon-btn {
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }

        .player-controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.5rem;
        }

        .control-buttons {
            display: flex;
            gap: 1rem;
        }
        
        .control-buttons .icon-btn {
            color: white;
            background: rgba(255, 255, 255, 0.2);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        #player-toggle-play {
            width: 50px;
            height: 50px;
            font-size: 1.2rem;
        }
        
        .control-buttons .icon-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.1);
        }

        .progress-container {
            display: flex;
            align-items: center;
            gap: 1rem;
            width: 100%;
        }

        .progress-bar {
            flex: 1;
            height: 4px;
            background-color: rgba(255, 255, 255, 0.3);
            border-radius: 2px;
            position: relative;
            cursor: pointer;
        }

        .progress {
            height: 100%;
            background-color: white;
            border-radius: 2px;
            width: 0%;
            position: relative;
        }

        .progress::after {
            content: '';
            position: absolute;
            right: -6px;
            top: -4px;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background-color: white;
            opacity: 0;
            transition: opacity 0.2s;
        }

        .progress-bar:hover .progress::after {
            opacity: 1;
        }

        .time {
            font-size: 0.9rem;
            color: #ccc;
            min-width: 45px;
            transition: color 0.5s ease;
            text-align: center;
        }

        .theme-selector {
            display: flex;
            gap: 1rem;
            margin-top: 1rem;
        }

        .theme-option {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            border: 3px solid transparent;
            transition: var(--transition);
        }

        .theme-option.active {
            border-color: var(--dark-color);
        }

        .theme-1 { background: linear-gradient(135deg, #6a11cb, #2575fc); }
        .theme-2 { background: linear-gradient(135deg, #ff6b6b, #ffa726); }
        .theme-3 { background: linear-gradient(135deg, #00b894, #00cec9); }
        .theme-4 { background: linear-gradient(135deg, #a29bfe, #fd79a8); }

        .file-actions {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        footer {
            text-align: center;
            padding: 2rem;
            margin-top: 3rem;
            color: #777;
            border-top: 1px solid #eee;
            margin-bottom: 100px; /* Espaço para o player fixo */
        }

        /* Notification styles */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 25px;
            border-radius: var(--border-radius);
            background: white;
            box-shadow: var(--box-shadow);
            z-index: 1000;
            animation: slideIn 0.3s ease-out;
        }

        .notification.info {
            background: linear-gradient(to right, var(--primary-color), var(--secondary-color));
            color: white;
        }

        .notification.error {
            background: linear-gradient(to right, #e74c3c, #c0392b);
            color: white;
        }

        @keyframes slideIn {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        /* --- Estilos do Modal de Letras --- */
        .lyrics-modal {
            display: none;
            position: fixed;
            z-index: 1001;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.6);
            animation: fadeIn 0.3s;
        }
        .lyrics-modal-content {
            background-color: #fefefe;
            margin: 10% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 80%;
            max-width: 600px;
            border-radius: var(--border-radius);
            position: relative;
        }
        body.dark-mode .lyrics-modal-content {
            background-color: #444;
            color: var(--text-color);
        }
        .lyrics-text {
            white-space: pre-wrap; /* Mantém as quebras de linha da letra */
            max-height: 60vh;
            overflow-y: auto;
            line-height: 1.8;
        }

        /* --- Estilos do Equalizador --- */
        .equalizer-panel {
            display: none;
            position: fixed;
            bottom: 110px; /* Acima do player */
            right: 20px;
            background-color: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            padding: 15px;
            box-shadow: var(--box-shadow);
            z-index: 1002;
            width: 380px;
            animation: fadeIn 0.3s;
        }
        body.dark-mode .equalizer-panel {
            background-color: rgba(50, 50, 50, 0.9);
            border-color: #555;
        }
        .equalizer-bands {
            display: flex;
            justify-content: space-around;
            align-items: flex-end;
            height: 120px;
            margin-bottom: 15px;
        }
        .band {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .band input[type="range"] {
            -webkit-appearance: none;
            appearance: none;
            width: 100px;
            height: 8px;
            transform: rotate(-90deg);
            cursor: pointer;
            background: #eee;
            border-radius: 4px;
        }
        body.dark-mode .band input[type="range"] {
            background: #555;
        }
        .band label {
            font-size: 0.75rem;
            margin-top: 5px;
        }
        .equalizer-presets {
            display: flex;
            gap: 8px;
            justify-content: center;
        }
        .equalizer-presets .btn-outline {
            padding: 5px 10px;
        }
        /* Search type select styles */
        #search-type {
            padding: 0.75rem;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            background-color: white;
            font-size: 1rem;
            min-width: 120px;
            cursor: pointer;
        }

        #search-type:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 2px rgba(106, 17, 203, 0.2);
        }

        .search-box {
            display: flex;
            gap: 1rem;
            margin-bottom: 1.5rem;
        }

        .no-results {
            text-align: center;
            padding: 2rem;
            color: #777;
            font-style: italic;
        }

        /* --- Responsividade --- */
        @media (max-width: 1024px) {
            .playlist-manager {
                grid-template-columns: 250px 1fr; /* Reduz a sidebar em tablets */
            }

            .album-creator, .playlist-creator {
                grid-template-columns: 1fr; /* Empilha em tablets */
            }

            .player-content {
                flex-direction: column;
                align-items: center;
            }
        }
        @media (max-width: 768px) {
            .header-content {
                flex-direction: column;
                gap: 1rem;
                padding: 1rem;
            }

            .logo {
                font-size: 1.5rem;
            }

            .logo i {
                font-size: 1.8rem;
            }

            nav ul {
                flex-direction: column;
                gap: 0.5rem;
                text-align: center;
                width: 100%;
            }

            nav a {
                padding: 0.5rem;
                display: block;
            }

            .user-profile {
                margin-top: 1rem;
                width: 100%;
                justify-content: center;
            }

            main {
                padding: 0 1rem;
                margin-top: 1rem;
            }

            .tab {
                flex: 1; /* Faz as abas ocuparem a largura disponível */
                text-align: center;
                padding: 0.8rem 1rem;
            }

            .tab-content {
                padding: 1.5rem;
            }

            .playlist-manager {
                grid-template-columns: 1fr;
                gap: 1.5rem;
            }

            .playlist-sidebar {
                border-right: none;
                border-bottom: 1px solid #eee;
                padding-right: 0;
                padding-bottom: 1rem;
                margin-bottom: 1rem;
            }

            .playlist-creator {
                flex-direction: column;
                grid-template-columns: 1fr; /* Garante que o editor de playlist também empilhe */
                gap: 1.5rem;
            }

            .search-box {
                flex-direction: column;
                gap: 0.8rem;
            }

            .search-box input,
            .search-box select,
            .search-box .btn {
                width: 100%;
            }

            .search-result-item {
                flex-direction: column;
                align-items: flex-start;
                gap: 0.8rem;
            }

            .search-result-item .track-info {
                width: 100%;
                justify-content: flex-start;
            }

            .search-result-item .track-actions {
                width: 100%;
                justify-content: flex-end; /* Alinha botões à direita */
            }

            .player-content {
                flex-direction: column;
                align-items: center;
                gap: 1rem;
            }

            .player-info {
                flex-direction: column;
                text-align: center;
            }

            .player-controls {
                flex-wrap: wrap;
                justify-content: center;
                gap: 0.8rem;
            }

            .progress-bar {
                width: 100%;
                margin: 0.5rem 0;
            }

            .control-buttons {
                margin-top: 0.5rem;
            }

            .file-actions {
                flex-direction: column;
                gap: 0.8rem;
            }
        }

        @media (max-width: 480px) {
            .logo {
                font-size: 1.3rem;
            }
            .logo i {
                font-size: 1.5rem;
            }
            .tab {
                padding: 0.6rem 0.5rem;
                font-size: 0.9rem;
            }
            .tab-content {
                padding: 1rem;
            }
            .section-title {
                font-size: 1.3rem;
            }
            .player-info {
                gap: 1rem;
            }
        }
    </style>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <!-- Importação das fontes do Google para a personalização -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&family=Lato:wght@400;700&family=Montserrat:wght@400;500;700&family=Open+Sans:wght@400;700&display=swap" rel="stylesheet">
</head>
<body>
    <header>
        <div class="header-content">
            <div class="logo">
                <i class="fas fa-music"></i>
                <span>musicore</span>
            </div>
            <nav>
                <ul>
                    <li id="nav-item-playlists" style="display: none;"><a href="#" data-tab="playlists">Editor de Playlists</a></li>
                    <li id="nav-item-search" style="display: none;"><a href="#" data-tab="search">Adicionar Músicas</a></li>
                    <li id="nav-item-personalize"><a href="#" class="active" data-tab="personalize">Editor de Site</a></li>
                    <li id="nav-item-community" style="display: none;"><a href="#" data-tab="community">Comunidade</a></li>
                </ul>
            </nav>
            <div id="user-profile-display" class="user-profile" style="display: none;">
                <span id="user-display-name"></span>
                <button id="btn-share-my-site" class="btn" title="Compartilhe seu site personalizado no feed da comunidade!">Meu site de música!</button>
            </div>
        </div>
    </header>

    <main>
        <div class="tab-container">
            <div class="tabs">            
                <div id="main-tab-personalize" class="tab active" data-tab="personalize">Personalizar seu Site</div>
                <div id="main-tab-playlists" class="tab" data-tab="playlists" style="display: none;">Editor de Playlists</div>
                <div id="main-tab-search" class="tab" data-tab="search" style="display: none;">Adicionar Músicas</div>
                <div id="main-tab-community" class="tab" data-tab="community" style="display: none;">Comunidade</div>
            </div>

            <div class="tab-content" id="playlists" style="display: none;">
                <h2 class="section-title">
                    <i class="fas fa-edit"></i>
                    Editor de Playlists
                </h2>
                <div class="playlist-manager">
                    <div class="playlist-sidebar">
                        <button id="btn-new-playlist" class="btn" style="margin-bottom: 1rem;">
                            <i class="fas fa-plus"></i> Nova Playlist
                        </button>
                        <div id="playlist-list-container">
                            <!-- Lista de playlists salvas -->
                        </div>
                    </div>
                    <div class="playlist-editor">
                        <div class="playlist-creator">
                            <div class="album-details">
                                <div class="form-group">
                                    <label for="playlist-name">Nome da Playlist</label>
                                    <input type="text" id="playlist-name" placeholder="Selecione ou crie uma nova">
                                </div>
                                <div class="form-group">
                                    <label for="playlist-description">Descrição</label>
                                    <textarea id="playlist-description" rows="3" placeholder="Descreva sua playlist"></textarea>
                                </div>
                                <button class="btn btn-save-playlist">
                                    <i class="fas fa-save"></i> Salvar Playlist
                                </button>
                            </div>
                            <div class="track-list">
                                <h3 class="section-title">
                                    <i class="fas fa-music"></i>
                                    Músicas da Playlist
                                </h3>
                                <!-- As faixas da playlist selecionada aparecerão aqui -->
                            </div>
                        </div>

                        <div class="file-actions">
                            <button class="btn btn-outline" onclick="exportToJson(currentPlaylist, 'playlist.json')">
                                <i class="fas fa-download"></i> Exportar JSON
                            </button>
                            <button class="btn btn-outline" onclick="document.getElementById('import-playlist-input').click()">
                                <i class="fas fa-upload"></i> Importar JSON
                            </button>
                            <button id="btn-add-to-spotify" class="btn" style="background:#1db954;">
                                <i class="fab fa-spotify"></i> Adicionar ao Spotify
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <div class="tab-content" id="community" style="display: none;">
                <h2 class="section-title">
                    <i class="fas fa-users"></i>
                    Feed da Comunidade
                </h2>
                <div id="community-feed-container" class="feed-grid"></div>
            </div>

            <div class="tab-content" id="search" style="display: none;">
                <h2 class="section-title">
                    <i class="fas fa-plus-circle"></i>
                    Adicionar Músicas
                </h2>
                <div class="search-section">
                    <div class="search-box">
      <input type="text" id="search-input" placeholder="Digite o nome da música ou artista">
                        <select id="search-type">
                            <option value="youtube">YouTube</option>
                            <option value="track">Música (Spotify)</option>
                            <option value="artist">Artista (Spotify)</option>
                            <option value="album">Álbum (Spotify)</option>                            
                            <option value="itunes">iTunes (Prévia)</option>
                            <option value="track">Música</option>
                        </select>
                        <button class="btn btn-search">
                            <i class="fas fa-search"></i> Buscar
                        </button>
                    </div>
                    <div class="search-results"></div>
                </div>
            </div>
        </div>

        <div class="tab-content active" id="personalize">
            <h2 class="section-title">
                <i class="fas-fa-palette"></i>
                Personalize seu Site
            </h2>
            <div class="personalization-options">
                <div class="form-group">
                    <label for="bg-image-url">URL da Imagem de Fundo</label>
                    <input type="text" id="bg-image-url" placeholder="Cole a URL de uma imagem aqui">
                </div>
                <div class="form-group">
                    <label for="button-size">Tamanho dos Botões</label>
                    <input type="range" id="button-size" min="0.8" max="1.5" step="0.1" value="1">
                </div>
                <div class="form-group">
                    <label for="font-select">Fonte do Site</label>
                    <select id="font-select">
                        <option value="'Segoe UI', Tahoma, Geneva, Verdana, sans-serif">Padrão (Segoe UI)</option>
                        <option value="'Roboto', sans-serif">Roboto</option>
                        <option value="'Lato', sans-serif">Lato</option>
                        <option value="'Montserrat', sans-serif">Montserrat</option>
                        <option value="'Open Sans', sans-serif">Open Sans</option>
                    </select>
                </div>
                <div class="form-group">
                    <button id="btn-apply-customization" class="btn">
                        <i class="fas fa-check"></i> Aplicar
                    </button>
                    <button id="btn-reset-customization" class="btn btn-outline">
                        <i class="fas fa-undo"></i> Resetar
                    </button>
                </div>
                <div class="form-group">
                    <label>Tema de Cores</label>
                    <div style="display: flex; gap: 10px; flex-wrap: wrap;">
                        <div>
                            <label for="primary-color-picker" style="font-size: 0.9em;">Cor Primária</label>
                            <input type="color" id="primary-color-picker" value="#6a11cb">
                        </div>
                        <div>
                            <label for="secondary-color-picker" style="font-size: 0.9em;">Cor Secundária</label>
                            <input type="color" id="secondary-color-picker" value="#2575fc">
                        </div>
                        <div>
                            <label for="accent-color-picker" style="font-size: 0.9em;">Cor de Destaque</label>
                            <input type="color" id="accent-color-picker" value="#ff6b6b">
                        </div>
                    </div>
                </div>
                <div class="form-group">
                    <label for="border-radius-slider">Raio da Borda (px)</label>
                    <input type="range" id="border-radius-slider" min="0" max="30" step="1" value="12">
                </div>
                <div class="form-group"><label class="switch" style="display: inline-flex; align-items: center; gap: 10px;"><input type="checkbox" id="dark-mode-toggle"><span style="cursor: pointer;">Ativar Modo Escuro</span></label></div>
                <hr style="margin: 2rem 0; border: none; border-top: 1px solid #eee;">
                <div class="form-group">
                    <label>Adicionar Abas ao seu Site</label>
                    <p style="font-size: 0.9em; color: #666; margin-top: -0.5rem; margin-bottom: 1rem;">Clique para adicionar seções ao menu de navegação.</p>
                    <div style="display: flex; gap: 10px; flex-wrap: wrap;">
                        <button id="btn-add-tab-playlists" class="btn btn-outline">Adicionar "Editor de Playlists"</button>
                        <button id="btn-add-tab-search" class="btn btn-outline">Adicionar "Busca de Músicas"</button>
                        <button id="btn-add-tab-community" class="btn btn-outline">Adicionar "Comunidade"</button>
                    </div>
                </div>
                <hr style="margin: 2rem 0; border: none; border-top: 1px solid #eee;">
                <div class="form-group">
                    <label>Compartilhe seu Site</label>
                    <p style="font-size: 0.9em; color: #666; margin-top: -0.5rem; margin-bottom: 1rem;">
                        Crie um nome de usuário para seu link personalizado. Outros poderão acessar sua criação com um link curto!
                    </p>
                    <div style="display: flex; gap: 10px;">
                        <input type="text" id="custom-link-slug" placeholder="Ex: daniel-music">
                        <button id="btn-save-and-share" class="btn"><i class="fas fa-save"></i> Salvar e Gerar Link</button>
                    </div>
                    <p id="link-status-message" style="font-size: 0.9em; margin-top: 0.5rem; min-height: 1.2em;"></p>
                    <div style="display: flex; gap: 10px; margin-top: 1rem;">
                        <input type="text" id="shareable-link-input" readonly style="background-color: #e9ecef; cursor: text;">
                        <button id="btn-copy-link" class="btn"><i class="fas fa-copy"></i> Copiar</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal para Postar no Feed -->
        <div id="post-feed-modal" class="modal" style="display: none;">
            <div class="modal-content">
                <span class="close-btn" id="close-post-modal">&times;</span>
                <h3>Publicar seu Tema no Feed</h3>
                <p>Dê um nome para a sua personalização. Outros usuários poderão ver e aplicar seu tema!</p>
                <div class="form-group" style="margin-top: 1rem;">
                    <label for="theme-name-input">Nome do Tema</label>
                    <input type="text" id="theme-name-input" placeholder="Ex: Meu Tema Darkwave">
                </div>
                <div id="post-error-message" style="color: var(--accent-color); font-size: 0.9em; margin-top: 0.5rem;"></div>
                <div style="display: flex; justify-content: flex-end; gap: 10px; margin-top: 1.5rem;">
                    <button id="btn-cancel-post" class="btn btn-outline">Cancelar</button>
                    <button id="btn-confirm-post" class="btn">Publicar</button>
                </div>
            </div>
        </div>


        <!-- Modal de Login do Firebase -->
        <div id="firebase-login-modal" class="login-modal">
            <div class="login-modal-content">
                <span class="close-btn" id="close-login-modal">&times;</span>
                <h2>Acesse ou Crie sua Conta</h2>
                <p>Salve suas playlists e personalizações na nuvem.</p>
                <br>
                <button id="google-login-btn" class="btn btn-google"><i class="fab fa-google"></i> Entrar com Google</button>
                <button id="spotify-login-btn" class="btn btn-spotify"><i class="fab fa-spotify"></i> Entrar com Spotify</button>
                
                <div class="login-separator">ou</div>

                <div class="form-group">
                    <label for="email-input">Email</label>
                    <input type="email" id="email-input" placeholder="seu@email.com">
                </div>
                <div class="form-group">
                    <label for="password-input">Senha</label>
                    <input type="password" id="password-input" placeholder="******">
                </div>
                <button id="email-login-btn" class="btn">Entrar com Email</button>
                <button id="email-register-btn" class="btn btn-outline">Registrar com Email</button>

                <div id="login-error-message"></div>
            </div>
        </div>

        <!-- Modal para Letras de Música -->
        <div id="lyrics-modal" class="lyrics-modal">
            <div class="lyrics-modal-content">
                <span class="close-btn" style="position: absolute; top: 10px; right: 20px; font-size: 28px; font-weight: bold; cursor: pointer;">&times;</span>
                <h3 id="lyrics-title" style="margin-bottom: 15px;">Letra da Música</h3>
                <div id="lyrics-text-container" class="lyrics-text">
                    <p>Buscando letra...</p>
                </div>
            </div>
        </div>

        <!-- Painel do Equalizador -->
        <div id="equalizer-panel" class="equalizer-panel">
            <h4 style="text-align: center; margin-bottom: 10px;">Equalizador</h4>
            <div id="equalizer-bands" class="equalizer-bands">
                <!-- Bandas serão geradas via JS -->
            </div>
            <div class="equalizer-presets">
                <button class="btn btn-outline" data-preset="flat">Flat</button>
                <button class="btn btn-outline" data-preset="pop">Pop</button>
                <button class="btn btn-outline" data-preset="rock">Rock</button>
                <button class="btn btn-outline" data-preset="jazz">Jazz</button>
                <button id="eq-reset-btn" class="btn btn-outline" title="Resetar"><i class="fas fa-undo"></i></button>
            </div>
            <small style="display: block; text-align: center; margin-top: 10px; font-size: 0.8em; color: #888;">
                *O equalizador funciona com YouTube e prévias do iTunes.
            </small>
        </div>











    <div id="fixed-player" class="player hidden">
        <div id="youtube-player-container" style="display: none; position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;"></div>
        <div class="player-content" style="position: relative; z-index: 2;">
            <div class="player-info">
                <img id="player-cover" src="https://via.placeholder.com/60" alt="Capa do Álbum">
                <div class="track-details">
                    <h4 id="player-title">Selecione uma música</h4>
                    <p id="player-info">...</p>
                </div>
            </div>
            <div class="player-controls">
                <div class="control-buttons">
                    <button id="player-previous" class="icon-btn"><i class="fas fa-step-backward"></i></button>
                    <button id="player-toggle-play" class="icon-btn"><i class="fas fa-play"></i></button>
                    <button id="player-next" class="icon-btn"><i class="fas fa-step-forward"></i></button>
                    <button id="player-lyrics" class="icon-btn" style="display: none;" title="Ver Letra"><i class="fas fa-microphone-alt"></i></button>
                    <button id="player-equalizer" class="icon-btn" title="Equalizador"><i class="fas fa-sliders-h"></i></button>
                </div>
                <div class="progress-container">
                    <span id="player-current-time" class="time">0:00</span>
                    <div id="player-progress-bar" class="progress-bar">
                        <div id="player-progress" class="progress"></div>
                    </div>
                    <span id="player-duration" class="time">0:00</span>
                </div>
            </div>
            <div class="player-ui-controls">
                <button id="player-expand-toggle" class="icon-btn" title="Expandir/Recolher"><i class="fas fa-chevron-up"></i></button>
                <button id="player-close" class="icon-btn" title="Fechar Player"><i class="fas fa-times"></i></button>
            </div>
        </div>
    </div>
    </main>

    <footer>
        <p>musicore &copy; 2024 - Crie e personalize suas playlists</p>
        <p><small>Uma ferramenta para dar vida às suas ideias musicais.</small></p>
    </footer>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/color-thief/2.3.2/color-thief.umd.js"></script>
    <!-- Firebase SDKs -->
    <script type="module">
        // Import Firebase modules (Modular SDK)
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, GoogleAuthProvider, signInWithPopup, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut, OAuthProvider } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-auth.js";
        import { getDatabase, ref, set, get } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-database.js";


        // --- Spotify auth + Web Playback SDK (PKCE) ---
        // NOTE: This implements a client-side Authorization Code with PKCE flow.
        // Requirements: create an app in https://developer.spotify.com and register a redirect URI.

    // Default client id is empty - user can enter a Client ID in the page input
    let SPOTIFY_CLIENT_ID = '71298793a0c64deca3e37a29f4e3c405';
    const SPOTIFY_REDIRECT_URI = window.location.origin + window.location.pathname; // registered redirect URI
    const SPOTIFY_SCOPES = [
            'streaming',
            'user-read-email',
            'user-read-private',
            'playlist-modify-private',
            'playlist-modify-public'
        ].join(' ');

        // token storage
        let spotifyAccessToken = null;
        let spotifyRefreshToken = null;
        let currentUser = null; // Armazena informações do usuário logado (Firebase Auth)

        // --- Firebase Config ---
        const firebaseConfig = {
            apiKey: "AIzaSyCC-Bmx4BVYU3KKLoo41zGFatBv59gsmo0",
            authDomain: "robo-9bb99.firebaseapp.com",
            databaseURL: "https://robo-9bb99-default-rtdb.firebaseio.com/",
            projectId: "robo-9bb99",
            storageBucket: "robo-9bb99.appspot.com",
            messagingSenderId: "786619724725",
            appId: "1:786619724725:web:ea8a14cb2cf20f7e469ffc"
        };

        const firebaseApp = initializeApp(firebaseConfig);
        const database = getDatabase(firebaseApp);
        const auth = getAuth(firebaseApp);


        async function sha256(plain) {
            const encoder = new TextEncoder();
            const data = encoder.encode(plain);
            const hash = await crypto.subtle.digest('SHA-256', data);
            return btoa(String.fromCharCode(...new Uint8Array(hash))).replace(/\+/g, '-').replace(/\//g, '_').replace(/=+$/, '');
        }

        function base64UrlEncode(buffer) {
            return btoa(String.fromCharCode(...new Uint8Array(buffer))).replace(/\+/g, '-').replace(/\//g, '_').replace(/=+$/, '');
        }

        async function generateCodeChallenge(codeVerifier) {
            const encoder = new TextEncoder();
            const data = encoder.encode(codeVerifier);
            const digest = await crypto.subtle.digest('SHA-256', data);
            return base64UrlEncode(digest);
        }

        function randomString(length) {
            const array = new Uint8Array(length);
            crypto.getRandomValues(array);
            return Array.from(array).map(b => ('0' + b.toString(16)).slice(-2)).join('');
        }

        // Start PKCE auth flow
        async function startSpotifyAuth() {
            // allow user to provide client id in an input (#spotify-client-id)
            const inputClientId = document.querySelector('#spotify-client-id')?.value?.trim();
            SPOTIFY_CLIENT_ID = inputClientId || SPOTIFY_CLIENT_ID;
            if (!SPOTIFY_CLIENT_ID) {
                alert('Insira o Client ID do seu app Spotify no campo "Client ID" antes de entrar.');
                return;
            }

            // generate code verifier and challenge
            const codeVerifier = base64UrlEncode(new TextEncoder().encode(randomString(64)));
            const codeChallenge = await generateCodeChallenge(codeVerifier);
            sessionStorage.setItem('spotify_code_verifier', codeVerifier);

            const params = new URLSearchParams({
                client_id: SPOTIFY_CLIENT_ID,
                response_type: 'code',
                redirect_uri: SPOTIFY_REDIRECT_URI,
                code_challenge_method: 'S256',
                code_challenge: codeChallenge,
                scope: SPOTIFY_SCOPES,
                show_dialog: 'true'
            });

            window.open(`https://accounts.spotify.com/authorize?${params.toString()}`, '_blank');
        } 

        // Exchange code for token using Spotify /api/token endpoint via CORS - Spotify does not allow PKCE token exchange from browser
        // Workaround: use the implicit flow for limited features or run a tiny server to exchange code securely.
        // Here we'll attempt an exchange but note: Spotify requires client secret for the token endpoint for Authorization Code flow.

        async function fetchWebApi(endpoint, method, body) {
            if (!spotifyAccessToken) {
                throw new Error('Spotify access token missing. Please login.');
            }
            const res = await fetch(`https://api.spotify.com/${endpoint}`, {
                headers: {
                    Authorization: `Bearer ${spotifyAccessToken}`,
                    'Content-Type': 'application/json'
                },
                method,
                body: body ? JSON.stringify(body) : undefined
            });
            return await res.json();
        }

        // Helper to parse redirect when returning with code
        (async function handleRedirectCallback(){
            const params = new URLSearchParams(window.location.search);
            const code = params.get('code');
            if (code) {
                const codeVerifier = sessionStorage.getItem('spotify_code_verifier');
                if (!codeVerifier) return;

                try {
                    // Exchange authorization code for tokens (PKCE) - Spotify supports this from client for PKCE
                    const body = new URLSearchParams({
                        grant_type: 'authorization_code',
                        code,
                        redirect_uri: SPOTIFY_REDIRECT_URI,
                        client_id: SPOTIFY_CLIENT_ID,
                        code_verifier: codeVerifier
                    });

                    const tokenRes = await fetch('https://accounts.spotify.com/api/token', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
                        body: body.toString()
                    });

                    if (!tokenRes.ok) {
                        const text = await tokenRes.text();
                        console.error('Token exchange failed', tokenRes.status, text);
                        showNotification('Falha ao obter token do Spotify. Veja console.');
                        history.replaceState({}, document.title, SPOTIFY_REDIRECT_URI);
                        return;
                    }

                    const tokenData = await tokenRes.json();
                    spotifyAccessToken = tokenData.access_token;
                    spotifyRefreshToken = tokenData.refresh_token;
                    const expiresIn = tokenData.expires_in || 3600;
                    const expiry = Date.now() + expiresIn * 1000;
                    localStorage.setItem('spotify_access_token', spotifyAccessToken);
                    localStorage.setItem('spotify_refresh_token', spotifyRefreshToken);
                    localStorage.setItem('spotify_token_expiry', expiry);

                    showNotification('Login Spotify concluído. Inicializando player...');
                    await initializeWebPlayback();
                    await linkSpotifyToFirebaseUser(); // Vincula ao usuário Firebase
                    
                } catch (err) {
                    console.error('Erro no callback do Spotify:', err);
                    showNotification('Erro no login Spotify. Veja console.');
                } finally {
                    history.replaceState({}, document.title, SPOTIFY_REDIRECT_URI);
                }
            } else {
                // try to restore token from session if present
                const stored = localStorage.getItem('spotify_access_token');
                if (stored) {
                    spotifyAccessToken = stored;
                    spotifyRefreshToken = localStorage.getItem('spotify_refresh_token');
                    const expiry = Number(localStorage.getItem('spotify_token_expiry')) || 0;
                    if (Date.now() > expiry - 60000 && spotifyRefreshToken) {
                        // token near expiry, refresh
                        await refreshSpotifyToken();
                    }
                    // initialize player without redirect
                    await initializeWebPlayback();
                    await linkSpotifyToFirebaseUser();
                }
            }
        })();

        // Refresh token when necessary
        async function refreshSpotifyToken() {
            if (!spotifyRefreshToken || !SPOTIFY_CLIENT_ID) return;
            try {
                const body = new URLSearchParams({
                    grant_type: 'refresh_token',
                    refresh_token: spotifyRefreshToken,
                    client_id: SPOTIFY_CLIENT_ID
                });
                const res = await fetch('https://accounts.spotify.com/api/token', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
                    body: body.toString()
                });
                const data = await res.json();
                spotifyAccessToken = data.access_token;
                const expiresIn = data.expires_in || 3600;
                const expiry = Date.now() + expiresIn * 1000;
                localStorage.setItem('spotify_access_token', spotifyAccessToken);
                localStorage.setItem('spotify_token_expiry', expiry);
                showNotification('Token Spotify renovado.');
            } catch (err) {
                console.error('Erro ao renovar token:', err);
            }
        }

        // Logout function
        async function doLogout() { 
            localStorage.removeItem('spotify_access_token');
            localStorage.removeItem('spotify_refresh_token');
            localStorage.removeItem('spotify_token_expiry');
            await signOut(auth); // Faz logout do Firebase
            window.location.reload();
        }

        // Initialize the Web Playback SDK and player
        let spotifyPlayer = null;
        let spotifyDeviceId = null;

        async function initializeWebPlayback() {
            if (!spotifyAccessToken) return;

            // load SDK if needed
            if (!window.Spotify) {
                await new Promise((resolve) => {
                    const s = document.createElement('script');
                    s.src = 'https://sdk.scdn.co/spotify-player.js';
                    s.onload = resolve;
                    document.head.appendChild(s);
                });
            }

            if (!window.Spotify) {
                console.error('Spotify Web Playback SDK não carregado');
                return;
            }

            spotifyPlayer = new window.Spotify.Player({
                name: 'musicore Player',
                getOAuthToken: cb => { cb(spotifyAccessToken); },
                volume: 0.8
            });

            // error handling
            spotifyPlayer.addListener('initialization_error', ({ message }) => { console.error(message); });
            spotifyPlayer.addListener('authentication_error', ({ message }) => { console.error(message); showNotification('Erro de autenticação no player'); });
            spotifyPlayer.addListener('account_error', ({ message }) => { console.error(message); showNotification('Conta inválida para reprodução (necessita Spotify Premium)'); });
            spotifyPlayer.addListener('playback_error', ({ message }) => { console.error(message); });

            // ready
            spotifyPlayer.addListener('ready', ({ device_id }) => {
                spotifyDeviceId = device_id;
                console.log('Device ID', device_id);
                showNotification('Player pronto. Você pode tocar músicas completas.');
                // transfer playback to this device (do not auto start)
                fetch('https://api.spotify.com/v1/me/player', {
                    method: 'PUT',
                    headers: { 'Authorization': `Bearer ${spotifyAccessToken}`, 'Content-Type': 'application/json' },
                    body: JSON.stringify({ device_ids: [spotifyDeviceId] })
                });
            });

            spotifyPlayer.addListener('not_ready', ({ device_id }) => {
                console.log('Device ID not ready', device_id);
            });

            // Player state changed
            spotifyPlayer.addListener('player_state_changed', state => {
                if (!state) {
                    return;
                }
                updatePlayerUIForActiveMedia(state);

                // O listener 'player_state_changed' já lida com a atualização da UI.
                // O intervalo de progresso é gerenciado dentro de updatePlayerUIForActiveMedia.
            });


            await spotifyPlayer.connect();
        }

        // --- Firebase Auth Functions ---
        onAuthStateChanged(auth, async (user) => {
            const loginButton = document.getElementById('main-login-button');
            const userProfileDisplay = document.getElementById('user-profile-display');

            if (user) {
                // Usuário está logado
                currentUser = {
                    uid: user.uid,
                    name: user.displayName || user.email.split('@')[0],
                    email: user.email
                };
                console.log("Usuário Firebase logado:", currentUser);

                document.getElementById('user-display-name').textContent = `Olá, ${currentUser.name}`;
                userProfileDisplay.style.display = 'flex';
                loginButton.style.display = 'none';
                document.getElementById('firebase-login-modal').style.display = 'none';

                // Carrega dados do usuário (playlists, customizações)
                await loadUserSettings(currentUser.uid);
                await loadSavedData();

            } else {
                // Usuário não está logado
                currentUser = null;
                userProfileDisplay.style.display = 'none';
                loginButton.style.display = 'flex';
                console.log("Nenhum usuário Firebase logado.");
                // Carrega dados locais (convidado)
                await loadSavedData();
            }
        });

        async function linkSpotifyToFirebaseUser() {
            if (!spotifyAccessToken || !currentUser) return;
            const spotifyUser = await fetchWebApi('v1/me', 'GET');
            // Salva o token do Spotify no perfil do usuário no Firebase
            const spotifyData = {
                spotifyId: spotifyUser.id,
                spotifyAccessToken,
                spotifyRefreshToken,
                spotifyTokenExpiry: localStorage.getItem('spotify_token_expiry')
            };
            await set(ref(database, `users/${currentUser.uid}/spotify`), spotifyData);
            showNotification('Conta Spotify vinculada com sucesso!');
        }

        async function loginWithGoogle() {
            const provider = new GoogleAuthProvider();
            try {
                await signInWithPopup(auth, provider);
            } catch (error) {
                document.getElementById('login-error-message').textContent = error.message;
            }
        }

        async function loginWithEmail() {
            const email = document.getElementById('email-input').value;
            const password = document.getElementById('password-input').value;
            if (!email || !password) {
                document.getElementById('login-error-message').textContent = 'Preencha e-mail e senha.';
                return;
            }
            try {
                await signInWithEmailAndPassword(auth, email, password);
            } catch (error) {
                document.getElementById('login-error-message').textContent = error.message;
            }
        }

        async function registerWithEmail() {
            const email = document.getElementById('email-input').value;
            const password = document.getElementById('password-input').value;
            if (!email || !password) {
                document.getElementById('login-error-message').textContent = 'Preencha e-mail e senha para registrar.';
                return;
            }
            try {
                await createUserWithEmailAndPassword(auth, email, password);
            } catch (error) {
                document.getElementById('login-error-message').textContent = error.message;
            }
        }

        function setupLoginModal() {
            const modal = document.getElementById('firebase-login-modal');
            const mainLoginButton = document.getElementById('main-login-button');
            document.getElementById('close-login-modal').addEventListener('click', () => modal.style.display = 'none');
            document.getElementById('google-login-btn').addEventListener('click', loginWithGoogle);
            document.getElementById('spotify-login-btn').addEventListener('click', startSpotifyAuth);
            document.getElementById('email-login-btn').addEventListener('click', loginWithEmail);
            document.getElementById('email-register-btn').addEventListener('click', registerWithEmail);
        }

        // Função para criar playlist no Spotify
        async function createSpotifyPlaylist(tracksUri) {
            try {
                const { id: user_id } = await fetchWebApi('v1/me', 'GET');

                const playlist = await fetchWebApi(
                    `v1/users/${user_id}/playlists`, 'POST', {
                        "name": document.querySelector('#playlist-name').value || "Minha playlist musicore",
                        "description": document.querySelector('#playlist-description').value || "Playlist criada via musicore",
                        "public": false
                    }
                );

                await fetchWebApi(
                    `v1/playlists/${playlist.id}/tracks?uris=${tracksUri.join(',')}`,
                    'POST'
                );

                showNotification('Playlist criada com sucesso no Spotify!');
                return playlist;
            } catch (error) {
                console.error('Erro ao criar playlist:', error);
                showNotification('Erro ao criar playlist no Spotify');
            }
        }
        
        // Configuração da API TheAudioDB
        const API_BASE_URL = 'https://www.theaudiodb.com/api/v1/json';
        let API_KEY = '1'; // Chave de API gratuita padrão para TheAudioDB
        let YOUTUBE_API_KEY = 'AIzaSyDfLDixV_rXI3VKhOM_tdq_ldUrdRdQSc8'; // Substitua pela sua chave de API do YouTube

        // Estado da aplicação
        let currentPlaylist = null;
        let audioPlayer = new Audio(); // Usado para previews do iTunes
        let currentPlayingMediaType = 'none'; // 'spotify', 'youtube', 'itunes', 'none'
        let currentPlayingTrackDetails = null; // Armazena detalhes do item atualmente em reprodução

        // Função para controlar o player do Spotify
        const playbackQueue = [];
        let queueIndex = -1;

        // Função para tocar uma música específica
        async function playSpotifyTrack(track) {
            if (!track) return;

            // Define os detalhes da faixa atual para Spotify
            currentPlayingTrackDetails = {
                id: track.id,
                name: track.name,
                artist: track.artists?.map(a => a.name).join(', ') || '',
                album: track.album?.name || '',
                thumb: track.album?.images?.[0]?.url || 'https://via.placeholder.com/60',
                duration_ms: track.duration_ms, // Spotify fornece duração em ms
                preview: track.preview_url // Adicionado para fallback
            };
            const trackId = track.id;

            // If Web Playback SDK player is available, use it
            if (spotifyPlayer && spotifyDeviceId && spotifyAccessToken) {
                try {
                    await fetch(`https://api.spotify.com/v1/me/player/play?device_id=${spotifyDeviceId}`, {
                        method: 'PUT',
                        headers: { 'Authorization': `Bearer ${spotifyAccessToken}`, 'Content-Type': 'application/json' },
                        body: JSON.stringify({ uris: [`spotify:track:${trackId}`] })
                        // body: JSON.stringify({ uris: [`spotify:track:${track.uri.split(':')[2]}`] }) // Usar URI completa para garantir, se track.id não for suficiente
                    });
                    showNotification('Tocando música...');

                    updatePlayerVisibility('spotify');

                    // O listener 'player_state_changed' chamará updatePlayerUIForActiveMedia
                    return;
                } catch (err) {
                    console.error('Erro ao tocar:', err);
                }
            } else if (currentPlayingTrackDetails.preview) {
                // Fallback para tocar a prévia de 30s se o player não estiver ativo
                showNotification('Tocando prévia de 30s. Conecte uma conta Premium para ouvir a música completa.');
                playItunesPreview(currentPlayingTrackDetails);
            } else {
                showNotification('Faça login com Spotify Premium para tocar músicas completas ou use o YouTube.', 'error');
            }
        }


        // Função para tocar próxima música da fila
        function playNext() {            
            if (queueIndex + 1 < playbackQueue.length) {
                queueIndex++;
                const track = playbackQueue[queueIndex];
                if (track.type === 'youtube') {
                    playYouTubeVideo(track.videoId, track);
                } else if (track.type === 'spotify') {
                    playSpotifyTrack(track);
                } else { // itunes
                    playItunesPreview(track);
                }
            } else {
                showNotification('Fim da fila de reprodução.', 'info');
                updatePlayerVisibility('none');
            }
        }

        // Função para tocar música anterior da fila
        function playPrevious() {
            if (queueIndex - 1 >= 0) {
                queueIndex--;
                const track = playbackQueue[queueIndex];
                if (track.type === 'youtube') {
                    playYouTubeVideo(track.videoId, track);
                } else if (track.type === 'spotify') {
                    playSpotifyTrack(track);
                } else { // itunes
                    playItunesPreview(track);
                }
            }
        }

        // Função para abrir no Spotify
        function openInSpotify(uri) {
            window.open(`https://open.spotify.com/track/${uri.split(':')[2]}`, '_blank');
        }

        // Toggle para mostrar/esconder o player incorporado (agora mais genérico)
        function toggleSpotifyPlayer() {
            // Esta função pode ser removida ou adaptada se o player fixo for o único player
            // Por enquanto, ela apenas mostra/esconde o player fixo.
            const fixedPlayer = document.getElementById('fixed-player');
            if (fixedPlayer.classList.contains('hidden')) {
                fixedPlayer.classList.remove('hidden');
            } else {
                fixedPlayer.classList.add('hidden');
            }
        }

        // Função para adaptar a cor do player à capa do álbum
        const colorThief = new ColorThief();
        const playerElement = document.getElementById('fixed-player');
        const tempImg = new Image();
        tempImg.crossOrigin = "Anonymous"; // Essencial para ler pixels de imagens de outro domínio

        tempImg.addEventListener('load', function() {
            try {
                const dominantColor = colorThief.getColor(tempImg);
                const rgb = `rgb(${dominantColor.join(',')})`;
                
                // Cria um gradiente com a cor dominante
                playerElement.style.background = `linear-gradient(to top, ${rgb}, rgba(0,0,0,0.7))`;

                // Verifica o brilho da cor para ajustar a cor do texto
                const brightness = Math.round(((parseInt(dominantColor[0]) * 299) +
                                             (parseInt(dominantColor[1]) * 587) +
                                             (parseInt(dominantColor[2]) * 114)) / 1000);
                const textColor = (brightness > 125) ? 'black' : 'white';
                const secondaryTextColor = (brightness > 125) ? '#333' : '#ccc';

                playerElement.querySelectorAll('#player-title, .control-buttons .icon-btn, .volume-control i').forEach(el => el.style.color = textColor);
                playerElement.querySelectorAll('#player-info, .time').forEach(el => el.style.color = secondaryTextColor);
                playerElement.querySelector('#player-progress').style.backgroundColor = textColor;
                playerElement.querySelector('#player-progress-bar').style.backgroundColor = `rgba(${textColor === 'white' ? '255,255,255' : '0,0,0'}, 0.3)`;

            } catch (e) {
                console.error("Erro ao extrair cor:", e);
                // Volta para o padrão em caso de erro
                playerElement.style.background = `linear-gradient(to top, rgba(0,0,0,0.8), transparent)`;
            }
        });

        // Função auxiliar para atualizar metadados da Media Session API
        function updateMediaSessionMetadata() {
            if ('mediaSession' in navigator && currentPlayingTrackDetails) {
                navigator.mediaSession.metadata = new MediaMetadata({
                    title: currentPlayingTrackDetails.name,
                    artist: currentPlayingTrackDetails.artist,
                    album: currentPlayingTrackDetails.album,
                    artwork: [
                        { src: currentPlayingTrackDetails.thumb, sizes: '96x96', type: 'image/png' },
                        { src: currentPlayingTrackDetails.thumb, sizes: '128x128', type: 'image/png' },
                        { src: currentPlayingTrackDetails.thumb, sizes: '192x192', type: 'image/png' },
                        { src: currentPlayingTrackDetails.thumb, sizes: '256x256', type: 'image/png' },
                        { src: currentPlayingTrackDetails.thumb, sizes: '384x384', type: 'image/png' },
                        { src: currentPlayingTrackDetails.thumb, sizes: '512x512', type: 'image/png' },
                    ]
                });
            }
        }
        function updatePlayerColor(imageUrl) {
            tempImg.src = imageUrl;
        }

        // Variáveis para controlar os intervalos de atualização de progresso
        let spotifyProgressInterval = null;
        let youtubeProgressInterval;

        // Função unificada para atualizar a visibilidade e o conteúdo do player
        function updatePlayerVisibility(mediaType) {
            const youtubeContainer = document.getElementById('youtube-player-container');
            const fixedPlayer = document.getElementById('fixed-player');

            // Limpa quaisquer intervalos de progresso existentes para evitar conflitos
            if (spotifyProgressInterval) clearInterval(spotifyProgressInterval);
            if (youtubeProgressInterval) clearInterval(youtubeProgressInterval);

            if (mediaType === 'youtube') {
                youtubeContainer.style.display = 'block';
                fixedPlayer.classList.remove('hidden');
            } else if (mediaType === 'spotify' || mediaType === 'itunes') {
                youtubeContainer.style.display = 'none';
                fixedPlayer.classList.remove('hidden');
            } else { // 'none'
                fixedPlayer.classList.add('hidden'); // Esconde o player completamente
            }
            currentPlayingMediaType = mediaType;
            updatePlayerUIForActiveMedia(); // Atualiza os detalhes visuais
        }

        // Função unificada para atualizar a UI do player fixo (capa, título, artista, etc.)
        function updatePlayerUIForActiveMedia(spotifyState = null) {
            const playerCover = document.getElementById('player-cover');
            const playerTitle = document.getElementById('player-title');
            const playerInfo = document.getElementById('player-info');
            const playIcon = document.getElementById('player-toggle-play').querySelector('i');
            const progressBar = document.getElementById('player-progress');
            const currentTimeSpan = document.getElementById('player-current-time');
            const durationSpan = document.getElementById('player-duration');

            if (currentPlayingTrackDetails) {
                // Atualiza metadados da Media Session API
                playerCover.src = currentPlayingTrackDetails.thumb;
                playerTitle.textContent = currentPlayingTrackDetails.name;
                playerInfo.textContent = currentPlayingTrackDetails.artist;
                document.getElementById('player-lyrics').style.display = 'inline-flex';
                updatePlayerColor(currentPlayingTrackDetails.thumb); // Atualiza a cor de fundo do player
            } else {
                // Estado padrão se não houver detalhes da faixa
                if ('mediaSession' in navigator) {
                    navigator.mediaSession.metadata = null;
                }

                // Limpa o intervalo de progresso do YouTube ao pausar
                if (youtubeProgressInterval) {
                    clearInterval(youtubeProgressInterval);
                    youtubeProgressInterval = null;
                }
                if (spotifyProgressInterval) {
                    clearInterval(spotifyProgressInterval);
                    spotifyProgressInterval = null;
                }
                playerCover.src = 'https://via.placeholder.com/60';
                playerTitle.textContent = 'Selecione uma música';
                playerInfo.textContent = '...';
                // Reseta a cor para o padrão se não houver faixa
                document.getElementById('player-lyrics').style.display = 'none';
                playerElement.style.background = `linear-gradient(to top, rgba(0,0,0,0.8), transparent)`;
                playerElement.querySelectorAll('#player-title, .control-buttons .icon-btn, .volume-control i').forEach(el => el.style.color = 'white');
                playerElement.querySelectorAll('#player-info, .time').forEach(el => el.style.color = '#ccc');
                playerElement.querySelector('#player-progress').style.backgroundColor = 'white';
                document.getElementById('player-progress-bar').style.backgroundColor = `rgba(255,255,255, 0.3)`;
            }

            // Atualiza o estado do botão play/pause e inicia/para intervalos de progresso
            if (currentPlayingMediaType === 'spotify' && spotifyPlayer && spotifyState && spotifyState.track_window) {
                const state = spotifyState; // O estado do player do Spotify
                playIcon.className = state.paused ? 'fas fa-play' : 'fas fa-pause'; // Atualiza o ícone de play/pause
                const progress = (state.position / state.duration) * 100; // Posição em porcentagem
                progressBar.style.width = `${progress}%`;
                currentTimeSpan.textContent = formatTime(state.position / 1000);
                durationSpan.textContent = formatTime(state.duration / 1000);

                // Atualiza detalhes da faixa do Spotify se mudou
                const currentTrack = state.track_window.current_track;
                if (!currentPlayingTrackDetails || currentPlayingTrackDetails.id !== currentTrack.id) {
                    currentPlayingTrackDetails = {
                        id: currentTrack.id,
                        name: currentTrack.name,
                        artist: currentTrack.artists.map(a => a.name).join(', '),
                        thumb: currentTrack.album.images[0].url
                    };
                    // Re-chama para atualizar a UI com os novos detalhes
                    playerCover.src = currentPlayingTrackDetails.thumb;
                    playerTitle.textContent = currentPlayingTrackDetails.name;
                    playerInfo.textContent = currentPlayingTrackDetails.artist;
                    updatePlayerColor(currentPlayingTrackDetails.thumb);
                    }
            } else if (currentPlayingMediaType === 'youtube' && youtubePlayer) {
                const playerState = youtubePlayer.getPlayerState();
                playIcon.className = (playerState === YT.PlayerState.PLAYING) ? 'fas fa-pause' : 'fas fa-play';
                if (playerState === YT.PlayerState.PLAYING && !youtubeProgressInterval) { // Inicia o intervalo apenas se não estiver rodando
                    youtubeProgressInterval = setInterval(updateYouTubePlayerProgress, 1000);
                }
            } else if (currentPlayingMediaType === 'itunes') {
                playIcon.className = audioPlayer.paused ? 'fas fa-play' : 'fas fa-pause';
                // Atualiza metadados da Media Session API para iTunes
                updateMediaSessionMetadata();
                // O progresso do iTunes é tratado pelo listener 'timeupdate' do audioPlayer
            }
        }

        // Função para lidar com o seek da Media Session API
        function handleSeek(seekOffset) {
            let currentTime, duration;
            if (currentPlayingMediaType === 'youtube' && youtubePlayer) {
                currentTime = youtubePlayer.getCurrentTime();
                duration = youtubePlayer.getDuration();
            } else if (currentPlayingMediaType === 'spotify' && spotifyPlayer) {
                // Para Spotify, precisamos obter o estado atual de forma assíncrona
                spotifyPlayer.getCurrentState().then(state => {
                    if (state) {
                        currentTime = state.position / 1000; // Convert ms to seconds
                        duration = state.duration / 1000; // Convert ms to seconds
                        const newTime = Math.max(0, Math.min(duration, currentTime + seekOffset));
                        seekInActivePlayer(newTime / duration);
                    }
                });
                return; // Sai da função, pois o Spotify é assíncrono
            } else if (currentPlayingMediaType === 'itunes') {
                currentTime = audioPlayer.currentTime;
                duration = audioPlayer.duration;
            } else {
                return; // Nenhum player ativo
            }

            const newTime = Math.max(0, Math.min(duration, currentTime + seekOffset));
            if (duration > 0) {
                seekInActivePlayer(newTime / duration);
            }
        }


        // Funções da API TheAudioDB
        async function fetchJSON(url) {
            try {
                console.log('Fazendo requisição para:', url);
                const res = await fetch(url, {
                    method: 'GET',
                    headers: {
                        'Accept': 'application/json',
                        'Content-Type': 'application/json',
                        'User-Agent': 'Mozilla/5.0'
                    },
                    mode: 'cors',
                    cache: 'no-cache'
                });
                
                if (!res.ok) {
                    console.error('Erro HTTP:', res.status, res.statusText);
                    throw new Error(`Erro na requisição: ${res.status} ${res.statusText}`);
                }
                
                const data = await res.json();
                console.log('Resposta da API:', data);
                
                if (!data) {
                    throw new Error('Resposta vazia da API');
                }
                
                return data;
            } catch (err) {
                console.error('Erro completo:', err);
                showNotification('Erro ao conectar: ' + err.message);
                return null;
            }
        }

        async function searchMusic() {
            console.log('Iniciando busca...');
            const searchInput = document.querySelector('#search-input').value.trim();
            const searchType = document.querySelector('#search-type').value;
            
            if (!searchInput.trim()) {
                showNotification('Por favor, digite um termo para buscar');
                return;
            }
            if (searchType === 'youtube') {
                searchYouTube(searchInput);
                return;
            }
            try {
                if (typeof spotifyAccessToken === 'string' && spotifyAccessToken) {
                    // Use Spotify Web API when we have a token
                    const response = await fetch(`https://api.spotify.com/v1/search?q=${encodeURIComponent(searchInput)}&type=${searchType}&limit=20`, {
                        headers: {
                            'Authorization': `Bearer ${spotifyAccessToken}`
                        }
                    });

                    if (!response.ok) {
                        const text = await response.text();
                        console.error('Spotify search error', response.status, text);
                        throw new Error('Erro na busca do Spotify');
                    }

                    const data = await response.json();
                    // Normalize results depending on type
                    if (searchType === 'track' && data.tracks && data.tracks.items.length) {
                        displaySpotifyResults(data.tracks.items);
                    } else if (searchType === 'album' && data.albums && data.albums.items.length) {
                        // Convert album items to a simplified structure
                        displaySpotifyResults(data.albums.items.map(a => ({
                            id: a.id,
                            uri: a.uri,
                            name: a.name,
                            album: a.name,
                            artists: a.artists || [],
                            album: { name: a.name },
                            album_obj: a
                        })));
                    } else if (searchType === 'artist' && data.artists && data.artists.items.length) {
                        displaySpotifyResults(data.artists.items.map(ar => ({
                            id: ar.id,
                            uri: ar.uri,
                            name: ar.name,
                            artists: [{ name: ar.name }],
                            album: { name: '' },
                            album_obj: ar
                        })));
                    } else {
                        showNotification('Nenhuma música encontrada');
                    }
                } else {
                    // Fallback to iTunes if not logged in to Spotify
                    searchItunes(searchInput);
                }
            } catch (error) {
                console.error('Erro na busca:', error);
                showNotification('Erro ao buscar. Tente novamente.');
            }
        }
        
        async function searchItunes(query) {
            try {
                const url = `https://itunes.apple.com/search?term=${encodeURIComponent(query)}&entity=song&limit=20&country=US`;
                const res = await fetch(url);
                if (!res.ok) throw new Error('iTunes search failed');
                const itunes = await res.json();
                if (itunes.results && itunes.results.length) {
                    displayItunesResults(itunes.results);
                } else {
                    showNotification('Nenhum resultado encontrado (iTunes)');
                }
            } catch (err) {
                console.error('Erro ao buscar no iTunes:', err);
                showNotification('Erro ao buscar no iTunes.', 'error');
            }
        }

         // --- Funções do YouTube ---
        let youtubePlayer;
        let isYouTubeApiReady = false;
        let videoToPlayAfterApiReady = null; // Guarda o vídeo se a API não estiver pronta

        // Carrega a API do YouTube de forma assíncrona
        function loadYouTubeAPI() {
            if (typeof YT === 'undefined' || typeof YT.Player === 'undefined') {
                const tag = document.createElement('script');
                tag.src = "https://www.youtube.com/iframe_api";
                document.head.appendChild(tag);
            }
        }

        // 1. Esta função é chamada AUTOMATICAMENTE pela API do YouTube quando ela carrega.
        function onYouTubeIframeAPIReady() {
            isYouTubeApiReady = true;
            // Se um vídeo foi clicado antes da API estar pronta, cria o player agora.
            if (videoToPlayAfterApiReady) {
                createYouTubePlayer(videoToPlayAfterApiReady.videoId);
                videoToPlayAfterApiReady = null; // Limpa a fila
            }
        }

        // 2. Função para criar o player
        function createYouTubePlayer(videoId) {
            // Se o player já existe, apenas carrega o novo vídeo.
            if (youtubePlayer) {
                youtubePlayer.loadVideoById(videoId);
                return;
            }

            // Cria um novo player. O 'onReady' vai confirmar que está pronto.
            youtubePlayer = new YT.Player('youtube-player-container', {
                height: '100%',
                width: '100%',
                videoId: videoId, // Carrega o vídeo inicial
                playerVars: { 'autoplay': 1, 'controls': 0, 'origin': window.location.origin },
                events: {
                    'onReady': onPlayerReady,
                    'onStateChange': onPlayerStateChange,
                    'onError': onPlayerError
                }
            });
        }

        // 3. Função chamada quando o player está pronto
        function onPlayerReady(event) {
            showNotification('Player do YouTube pronto.');
            // O autoplay:1 já deve iniciar o vídeo.
        }

        // 4. Função chamada quando o estado do player muda (tocando, pausado, etc)
        function onPlayerStateChange(event) {
            const playIcon = document.getElementById('player-toggle-play').querySelector('i');
            if (event.data == YT.PlayerState.PLAYING) {
                playIcon.className = 'fas fa-pause';
                if (youtubeProgressInterval) clearInterval(youtubeProgressInterval);
                youtubeProgressInterval = setInterval(updateYouTubePlayerProgress, 1000);
            } else if (event.data == YT.PlayerState.ENDED) {
                // Quando o vídeo do YouTube terminar, toque o próximo da fila.
                playNext();
            } else { // Pausado, finalizado, etc.
                playIcon.className = 'fas fa-play';
                if (youtubeProgressInterval) clearInterval(youtubeProgressInterval);
            }
            updatePlayerUIForActiveMedia();
        }

        // 5. Função para lidar com erros
        function onPlayerError(event) {
            console.error('Erro no Player do YouTube:', event.data);
            let errorMessage = 'Ocorreu um erro ao tentar reproduzir o vídeo.';
            if (event.data === 101 || event.data === 150) {
                errorMessage = 'Este vídeo não pode ser reproduzido aqui. Tente outra música.';
            } else if (event.data === 100) {
                errorMessage = 'Vídeo não encontrado. Pode ter sido removido.';
            }
            showNotification(errorMessage, 'error');
            updatePlayerVisibility('none');
        }

        // 6. Função principal para tocar o vídeo, que organiza tudo
        function playYouTubeVideo(videoId, trackDetails) {
            updatePlayerVisibility('youtube');
            currentPlayingTrackDetails = { ...trackDetails, videoId: videoId, type: 'youtube' };
            showNotification('Carregando vídeo do YouTube...');

            loadYouTubeAPI(); // Garante que a API está sendo carregada
            if (!isYouTubeApiReady) {
                // A API do YouTube ainda não carregou. Coloca o vídeo na "fila de espera".
                videoToPlayAfterApiReady = { 
                    videoId, 
                    trackDetails: { 
                        ...trackDetails, 
                        videoId: videoId, 
                        type: 'youtube' 
                    } 
                };
                showNotification('Inicializando player do YouTube...', 'info');
                return;
            }

            // A API está pronta, então podemos prosseguir.
            if (!youtubePlayer) {
                // Se o player não foi criado, cria agora.
                createYouTubePlayer(videoId);
            } else {
                // Se o player já existe, apenas carrega o novo vídeo.
                youtubePlayer.loadVideoById(videoId);
            }
        }

        function updateYouTubePlayerProgress() {
            if (youtubePlayer && youtubePlayer.getPlayerState() === YT.PlayerState.PLAYING) {
                const currentTime = youtubePlayer.getCurrentTime();
                const duration = youtubePlayer.getDuration();
                if (duration > 0) {
                    document.getElementById('player-progress').style.width = `${(currentTime / duration) * 100}%`;
                    document.getElementById('player-current-time').textContent = formatTime(currentTime);
                    document.getElementById('player-duration').textContent = formatTime(duration);
                }
            }
        }

        async function searchYouTube(query) {
            if (!YOUTUBE_API_KEY) {
                showNotification('A chave da API do YouTube não está configurada.', 'error');
                return;
            }
            // Adicionado 'videoEmbeddable=true' para filtrar apenas vídeos que podem ser incorporados
            const url = `https://www.googleapis.com/youtube/v3/search?part=snippet&q=${encodeURIComponent(query)}&type=video&videoEmbeddable=true&videoCategoryId=10&maxResults=15&key=${YOUTUBE_API_KEY}`;

            try {
                const response = await fetch(url);
                if (!response.ok) throw new Error('Falha na busca de vídeos do YouTube.');
                const data = await response.json();
                displayYouTubeResults(data.items || []);
            } catch (error) {
                console.error('Erro ao buscar no YouTube:', error);
                showNotification(error.message, 'error');
            }
        }

        function displayYouTubeResults(items) {
            const resultsContainer = document.querySelector('.search-results');
            if (!items || items.length === 0) {
                resultsContainer.innerHTML = '<p class="no-results">Nenhum vídeo encontrado no YouTube.</p>';
                return;
            }
            resultsContainer.innerHTML = items.map(item => {
                if (!item.id.videoId) return ''; // Pula resultados que não são vídeos (ex: canais)
                return `
                <div class="search-result-item" data-video-id="${item.id.videoId}" data-track-title="${item.snippet.title}" data-channel-title="${item.snippet.channelTitle}" data-thumbnail-url="${item.snippet.thumbnails.default.url}">
                    <div class="track-info">
                        <img src="${item.snippet.thumbnails.default.url}" alt="${item.snippet.title}" style="width: 60px; height: 60px; border-radius: 4px; margin-right: 15px;">
                        <div>
                            <h4>${item.snippet.title}</h4>
                            <p>${item.snippet.channelTitle}</p>
                        </div>
                    </div>
                    <div class="track-actions">
                        <button class="icon-btn play-youtube-video" title="Tocar no Player"><i class="fas fa-play"></i></button>
                        <button class="icon-btn add-to-collection-btn" title="Adicionar à coleção"><i class="fas fa-plus"></i></button>
                    </div>
                </div>
            `}).join('');

            resultsContainer.querySelectorAll('.play-youtube-video').forEach(button => {
                button.addEventListener('click', (event) => {
                    const itemEl = event.currentTarget.closest('.search-result-item');
                    const videoId = itemEl.dataset.videoId;
                    const trackDetails = { 
                        name: itemEl.dataset.trackTitle, 
                        artist: itemEl.dataset.channelTitle, 
                        thumb: itemEl.dataset.thumbnailUrl, 
                        album: 'YouTube' 
                    };
                    playYouTubeVideo(videoId, trackDetails);
                });
            });
        }


        function displaySpotifyResults(tracks) {
            const resultsContainer = document.querySelector('.search-results');
            resultsContainer.innerHTML = '';
            
            tracks.forEach(track => {
                const trackElement = document.createElement('div');
                trackElement.className = 'search-result-item';
                
                const imageUrl = track.album?.images?.[0]?.url || 'https://via.placeholder.com/60';
                trackElement.innerHTML = `
                    <div class="track-info">
                    <div class="track-actions">
                        <button class="icon-btn play-track" title="Tocar no Player">
                            <i class="fas fa-play"></i> 
                        </button>
                        <button class="icon-btn play-youtube" title="Ouvir no YouTube" style="display: none; color: #FF0000;">
                            <i class="fab fa-youtube"></i>
                        </button>
                        <button class="icon-btn add-to-playlist" data-track-uri="${track.uri}" title="Adicionar à playlist">
                            <i class="fas fa-plus"></i>
                        </button>
                    </div>
                    <div class="track-details">
                        <img src="${imageUrl}" alt="${track.name}" style="width: 60px; height: 60px; border-radius: 4px; margin-right: 15px;">
                        <div>
                            <h4>${track.name}</h4>
                            <p>${track.artists?.map(artist => artist.name).join(', ')} • ${track.album?.name || ''}</p>
                        </div>
                    </div>
                `;

                // Adicionar evento para tocar a música no player do Spotify
                const playButton = trackElement.querySelector('.play-track');
                playButton.addEventListener('click', () => playSpotifyTrack(track));

                // Encontra o vídeo no YouTube e adiciona o botão
                const youtubeBtn = trackElement.querySelector('.play-youtube');
                if (track.artists && track.artists.length > 0) {
                    findYouTubeVideoForTrack(track.name, track.artists[0].name).then(videoInfo => {
                        if (videoInfo && videoInfo.videoId) {
                            const trackDetails = {
                                name: videoInfo.title,
                                artist: videoInfo.channelTitle,
                                thumb: videoInfo.thumbnailUrl
                            };
                            youtubeBtn.style.display = 'inline-flex';
                            youtubeBtn.onclick = () => {
                                playYouTubeVideo(videoInfo.videoId, trackDetails);
                            };
                        }
                    });
                }

                // Adicionar evento para adicionar música à playlist
                const addButton = trackElement.querySelector('.add-to-playlist');
                addButton.addEventListener('click', () => {
                    const trackUri = track.uri;
                    if (!selectedTracks.includes(trackUri)) {
                        selectedTracks.push(trackUri);
                        showNotification('Música adicionada à seleção');
                        addButton.style.color = '#1DB954'; // Verde do Spotify
                    } else {
                        selectedTracks = selectedTracks.filter(uri => uri !== trackUri);
                        showNotification('Música removida da seleção');
                        addButton.style.color = '#777';
                    }
                });

                resultsContainer.appendChild(trackElement);
            });
        }

        async function findYouTubeVideoForTrack(trackName, artistName) {
            const query = `${trackName} ${artistName} official audio`;
            const url = `https://www.googleapis.com/youtube/v3/search?part=snippet&q=${encodeURIComponent(query)}&type=video&videoCategoryId=10&maxResults=1&key=${YOUTUBE_API_KEY}`;
            try {
                const response = await fetch(url);
                if (!response.ok) return null;
                const data = await response.json();
                if (data.items && data.items.length > 0) {
                    const item = data.items[0];
                    return { videoId: item.id.videoId, title: item.snippet.title, channelTitle: item.snippet.channelTitle, thumbnailUrl: item.snippet.thumbnails.default.url };
                }
                return null;
            } catch (error) {
                console.error("Erro ao buscar vídeo no YouTube:", error);
                return null;
            }
        }

        function displayItunesResults(results) { // Renomeado de displayItunesResults
            const resultsContainer = document.querySelector('.search-results');
            resultsContainer.innerHTML = '';

            results.forEach(item => {
                const trackElement = document.createElement('div');
                trackElement.className = 'search-result-item';
                const imageUrl = item.artworkUrl100 || 'https://via.placeholder.com/60';
                const name = item.trackName || item.collectionName || item.artistName;
                const artist = item.artistName || '';
                const album = item.collectionName || '';
                const preview = item.previewUrl || null;

                trackElement.innerHTML = `
                    <div class="track-info">
                        <img src="${imageUrl}" alt="${name}" style="width: 60px; height: 60px; border-radius: 4px; margin-right: 15px; object-fit:cover;">
                        <div>
                            <h4>${name}</h4>
                            <p>${artist} • ${album}</p>
                        </div>
                    </div>
                    <div class="track-actions">
                        ${preview ? `<button class="icon-btn itunes-play"><i class="fas fa-play"></i></button>` : '<span style="color:#777">Prévia indisponível</span>'}
                        <button class="icon-btn itunes-open" onclick="window.open(\'${item.trackViewUrl}\',\'_blank\')"><i class="fab fa-apple"></i></button>
                    </div>
                `;

                if (preview) {
                    const btn = trackElement.querySelector('.itunes-play');
                    btn.addEventListener('click', () => {
                        playItunesPreview({ // Usa a nova função para iTunes
                            name: name,
                            artist: artist,
                            album: album,
                            thumb: imageUrl,
                            preview: preview
                        });
                    });
                }


                resultsContainer.appendChild(trackElement);
            });
        }

        // Funções de manipulação de playlists
        async function savePlaylist() {
            const playlistName = document.querySelector('#playlist-name').value.trim();
            if (!playlistName) {
                showNotification('Por favor, insira um nome para a playlist.');
                return;
            }

            let playlists = [];
            if (currentUser && currentUser.uid) {
                const snapshot = await get(ref(database, `users/${currentUser.uid}/playlists`));
                if (snapshot.exists()) playlists = snapshot.val();
            } else { // Modo convidado
                playlists = JSON.parse(localStorage.getItem('playlists') || '[]');
            }
            
            if (currentPlaylist && currentPlaylist.createdAt) { // Atualizando playlist existente
                const index = playlists.findIndex(p => p.createdAt === currentPlaylist.createdAt);
                if (index > -1) {
                    playlists[index].name = playlistName;
                    playlists[index].description = document.querySelector('#playlist-description').value;
                    playlists[index].tracks = currentPlaylist.tracks; // Garante que as faixas estão atualizadas
                    currentPlaylist = playlists[index];
                }
            } else {
                // Criando nova playlist
                currentPlaylist = {
                    name: playlistName,
                    description: document.querySelector('#playlist-description').value,
                    tracks: currentPlaylist ? currentPlaylist.tracks : [], // Mantém faixas se já houver
                    createdAt: new Date().toISOString()
                };
                playlists.push(currentPlaylist);
            }

            if (currentUser && currentUser.uid) {
                await set(ref(database, `users/${currentUser.uid}/playlists`), playlists);
            } else {
                localStorage.setItem('playlists', JSON.stringify(playlists));
            }
            showNotification('Playlist salva com sucesso!');
            renderPlaylistList();

            // Auto-seleciona a playlist na lista
            setTimeout(() => {
                const item = document.querySelector(`.playlist-list-item[data-created-at="${currentPlaylist.createdAt}"]`);
                if (item) {
                    document.querySelectorAll('.playlist-list-item').forEach(el => el.classList.remove('active'));
                    item.classList.add('active');
                }
            }, 100);

            // Pergunta se quer adicionar ao Spotify
            if (spotifyAccessToken) {
                const spotifyTracks = currentPlaylist.tracks.filter(t => t.type === 'spotify' && t.uri);
                if (spotifyTracks.length > 0) {
                    if (confirm(`Deseja criar a playlist "${playlistName}" na sua conta do Spotify?`)) {
                        const trackUris = spotifyTracks.map(t => t.uri);
                        await createSpotifyPlaylist(trackUris);
                    }
                }
            }
        }

        async function togglePlay() {
            if (currentPlayingMediaType === 'youtube' && youtubePlayer) {
                const playerState = youtubePlayer.getPlayerState();
                if (playerState === YT.PlayerState.PLAYING || playerState === YT.PlayerState.BUFFERING) {
                    youtubePlayer.pauseVideo();
                    // Limpa o intervalo de progresso do YouTube ao pausar
                    if (youtubeProgressInterval) clearInterval(youtubeProgressInterval);
                } else { // PAUSED, ENDED, CUED
                    youtubePlayer.playVideo();
                }
            } else if (currentPlayingMediaType === 'spotify' && spotifyPlayer) {
                const state = await spotifyPlayer.getCurrentState();
                if (state && !state.paused) spotifyPlayer.pause();
                else if (state && state.paused) spotifyPlayer.resume();
                else spotifyPlayer.togglePlay();
            } else {
                // Fallback para o player de áudio se nenhum dos outros estiver ativo
                if (audioPlayer.paused) audioPlayer.play();
                else audioPlayer.pause(); // Pausa o player de áudio
            }
        }

        function seekInActivePlayer(progressRatio) {
            if (currentPlayingMediaType === 'youtube' && youtubePlayer) {
                const duration = youtubePlayer.getDuration();
                if (duration > 0) youtubePlayer.seekTo(duration * progressRatio, true);
            } else if (currentPlayingMediaType === 'spotify' && spotifyPlayer) {
                spotifyPlayer.getCurrentState().then(state => {
                    if (state) {
                        const duration = state.duration; // em ms
                        spotifyPlayer.seek(duration * progressRatio);
                    }
                });
            } else if (currentPlayingMediaType === 'itunes') {
                const duration = audioPlayer.duration; // em segundos
                if (isFinite(duration)) audioPlayer.currentTime = duration * progressRatio;
            }
        }

        // Called from search results when clicking Play on a result
        async function playTrackResult(item) {
            // item may have preview field already
            if (item.type !== 'track') {
                showNotification('Apenas faixas podem ser tocadas diretamente.', 'info');
                return;
            }

            if (item.preview) {
                playbackQueue.length = 0;
                playbackQueue.push(item); // Adiciona o item completo para a fila
                queueIndex = 0;
                playItunesPreview(item); // Usa a nova função para iTunes
                return;
            }

            // Try to lookup full track info by id to get preview
            if (item.id) {
                const apiKey = API_KEY || '123';
                const endpoint = `${API_BASE_URL}/${apiKey}/track.php?h=${item.id}`;
                const data = await fetchJSON(endpoint);
                if (data && data.track && data.track[0]) {
                    const t = data.track[0];
                    const prepared = {
                        id: t.idTrack,
                        name: t.strTrack,
                        artist: t.strArtist,
                        album: t.strAlbum,
                        thumb: t.strTrackThumb,
                        preview: t.strPreview || t.strMusicVid || null
                    };
                    playbackQueue.length = 0;
                    playbackQueue.push(prepared);
                    queueIndex = 0;
                    playItunesPreview(prepared); // Usa a nova função para iTunes
                    return;
                }
            }

            showNotification('Não foi possível tocar esta faixa', 'error');
        }

        // Play entire album by fetching tracks from lookup album endpoint
        async function playAlbum(id) {
            const apiKey = API_KEY || '123';
            const endpoint = `${API_BASE_URL}/${apiKey}/lookupalbum.php?m=${id}`;
            const data = await fetchJSON(endpoint);
            if (data && data.album) {
                // album info present, now fetch tracks
                const tracksEndpoint = `${API_BASE_URL}/${apiKey}/track.php?m=${id}`;
                const tdata = await fetchJSON(tracksEndpoint);
                if (tdata && tdata.track) {
                    playbackQueue.length = 0;
                    tdata.track.forEach(t => {
                        playbackQueue.push({
                            id: t.idTrack,
                            name: t.strTrack,
                            artist: t.strArtist,
                            album: t.strAlbum,
                            thumb: t.strTrackThumb,
                            preview: t.strPreview || t.strMusicVid || null
                        });
                    });
                    // start from first available preview
                    queueIndex = playbackQueue.findIndex(t => t.preview);
                    if (queueIndex === -1) {
                        showNotification('Nenhuma faixa com preview disponível no álbum', 'error');
                        return;
                    }
                    playItunesPreview(playbackQueue[queueIndex]);
                    return;
                }
            }
            showNotification('Não foi possível carregar o álbum', 'error');
        }

        // Play all tracks of an artist (uses artist id to get albums then tracks)
        async function playArtist(id) {
            const apiKey = API_KEY || '123';
            // get albums by artist
            const albumsEndpoint = `${API_BASE_URL}/${apiKey}/album.php?i=${id}`;
            const adata = await fetchJSON(albumsEndpoint);
            if (!adata || !adata.album) {
                showNotification('Nenhum álbum encontrado para este artista', 'error');
                return;
            }

            playbackQueue.length = 0;
            // For each album, fetch tracks
            for (const alb of adata.album) {
                const tracksEndpoint = `${API_BASE_URL}/${apiKey}/track.php?m=${alb.idAlbum}`;
                const tdata = await fetchJSON(tracksEndpoint);
                if (tdata && tdata.track) {
                    tdata.track.forEach(t => {
                        playbackQueue.push({
                            id: t.idTrack,
                            name: t.strTrack,
                            artist: t.strArtist,
                            album: t.strAlbum,
                            thumb: t.strTrackThumb,
                            preview: t.strPreview || t.strMusicVid || null
                        });
                    });
                }
            }

            if (playbackQueue.length === 0) {
                showNotification('Nenhuma faixa encontrada para este artista', 'error');
                return;
            }

            queueIndex = playbackQueue.findIndex(t => t.preview);
            if (queueIndex === -1) {
                showNotification('Nenhuma faixa com preview disponível', 'error');
                return;
            }
            playItunesPreview(playbackQueue[queueIndex]);
        }

        // Playback queue and controls for iTunes previews
   
        function addToPlaybackQueue(track) {
            playbackQueue.push(track);
            showNotification(`'${track.name}' adicionada à fila de reprodução.`);

            // Se nada estiver tocando, inicia a reprodução da música recém-adicionada.
            const isPlaying = (currentPlayingMediaType === 'youtube' && youtubePlayer && youtubePlayer.getPlayerState() === YT.PlayerState.PLAYING) ||
                              (currentPlayingMediaType === 'itunes' && !audioPlayer.paused);
            if (!isPlaying) {
                startPlaybackFromList(playbackQueue, playbackQueue.length - 1);
            }
        }

        function playItunesPreview(track) {
            if (!track.preview) {
                showNotification('Preview não disponível para esta faixa.', 'error');
                updatePlayerVisibility('none');
                return;
            }
            currentPlayingTrackDetails = track; // Update current playing track details
            audioPlayer.src = track.preview; // Assume 'preview' for iTunes
            audioPlayer.play();
            updatePlayerVisibility('itunes'); // Mostra a UI do player do iTunes

        }

        // Funções de utilitário
        function showNotification(message, type = 'info') {
            console.log('Notificação:', message, type);
            const notification = document.createElement('div');
            notification.className = `notification ${type}`;
            notification.style.backgroundColor = type === 'error' ? '#ff4444' : '#4CAF50';
            notification.style.color = 'white';
            notification.style.padding = '15px 30px';
            notification.style.borderRadius = '5px';
            notification.style.zIndex = '9999';
            notification.textContent = message;
            document.body.appendChild(notification);
            
        }

        function updateProgress() {
            const duration = isFinite(audioPlayer.duration) ? audioPlayer.duration : 0;
            const progress = duration ? (audioPlayer.currentTime / duration) * 100 : 0;
            document.querySelector('.progress').style.width = `${progress}%`;
            document.querySelector('.time').textContent = 
                `${formatTime(audioPlayer.currentTime)} / ${duration ? formatTime(duration) : '0:00'}`;
        }

        function importFromJson(file, callback) {
            const reader = new FileReader();
            reader.onload = (e) => {
                try {
                    const data = JSON.parse(e.target.result);
                    callback(data);
                } catch (error) {
                    showNotification('Erro ao importar arquivo. Formato inválido.', 'error');
                }
            };
            reader.readAsText(file);
        }
        
        function exportToJson(data, filename) {
            if (!data) {
                showNotification('Não há dados para exportar.', 'error');
                return;
            }
            const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            a.click();
            URL.revokeObjectURL(url);
        }

        function updatePlaylistView() {
            if (!currentPlaylist) return;
            
            document.querySelector('#playlist-name').value = currentPlaylist.name;
            document.querySelector('#playlist-description').value = currentPlaylist.description;
            updateTrackList(currentPlaylist.tracks, 'playlists'); // Contexto correto
        }
        
        function updateTrackList(tracks, context) {
            const container = document.querySelector(`#${context} .track-list`);
            container.innerHTML = tracks.map((track, index) => `
                <div class="track-item" data-track-id="${track.id}">
                    <div class="track-info">
                        <span class="track-number">${index + 1}</span>
                        <div>
                            <h4>${track.name}</h4>
                            <p>${track.artist}</p>
                        </div>
                    </div>
                    <div class="track-actions">
                        <button class="icon-btn play-from-list-btn" data-index="${index}">
                            <i class="fas fa-play"></i>
                        </button>
                        <button class="icon-btn remove-track-btn" data-track-id="${track.id}" data-context="${context}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                </div>
            `).join('');

            // Adiciona event listeners aos novos botões
            container.querySelectorAll('.play-from-list-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    const index = parseInt(btn.dataset.index, 10);
                    startPlaybackFromList(currentPlaylist.tracks, index);
                });
            });

            container.querySelectorAll('.remove-track-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    removeTrack(btn.dataset.trackId, btn.dataset.context);
                });
            });
        }
        
        function removeTrack(trackId, context) {
            if (context === 'playlists' && currentPlaylist) {
                const trackIdStr = String(trackId);
                currentPlaylist.tracks = currentPlaylist.tracks.filter(t => String(t.id) !== trackIdStr);
                updatePlaylistView();
                showNotification('Faixa removida da playlist.');
            } 
        }


        function displaySearchResults(data) {
            const resultsContainer = document.querySelector('.search-results');
            if (!data || !data.items || data.items.length === 0) {
                resultsContainer.innerHTML = '<p class="no-results">Nenhum resultado encontrado</p>';
                return;
            }

            resultsContainer.innerHTML = data.items.map(item => {
                let details = '';
                switch(item.type) {
                    case 'track':
                        details = `${item.artist} • ${item.album || 'Álbum desconhecido'}`;
                        break;
                    case 'artist':
                        details = item.bio ? item.bio.substring(0, 100) + '...' : '';
                        break;
                    case 'album':
                        details = `${item.artist} • ${item.year || 'Ano desconhecido'}`;
                        break;
                }
                // buttons: Play, Add
                const playBtn = (function() {
                    if (item.type === 'track') return `<button class="btn" onclick='playTrackResult(${JSON.stringify(item)})'><i class="fas fa-play"></i> Tocar</button>`;
                    if (item.type === 'album' && item.id) return `<button class="btn" onclick='playAlbum(${JSON.stringify(item.id)})'><i class="fas fa-compact-disc"></i> Tocar Álbum</button>`;
                    if (item.type === 'artist' && item.id) return `<button class="btn" onclick='playArtist(${JSON.stringify(item.id)})'><i class="fas fa-user"></i> Tocar Artista</button>`;
                    return '';
                })();

                return `
                    <div class="search-result-item">
                        <div class="track-info">
                            ${item.thumb ? `<img src="${item.thumb}" alt="${item.name}" style="width: 50px; height: 50px; object-fit: cover; margin-right: 10px;">` : ''}
                            <div>
                                <h4>${item.name}</h4>
                                <p>${details}</p>
                            </div>
                        </div>
                        <div style="display:flex; gap:8px; align-items:center;">
                            ${playBtn}
                            <button class="btn btn-outline" onclick='addToCurrentCollection(${JSON.stringify(item)})'>
                                <i class="fas fa-plus"></i> Adicionar
                            </button>
                        </div>
                    </div>
                `;
            }).join('');
        }

        // Add item to current album or playlist (default album)
        function addToCurrentCollection(item) {
            let track;
            const activeTab = document.querySelector('.tab.active').dataset.tab;

            // Verifica se o item vem do YouTube
            if (item.videoId) {
                track = {
                    id: item.videoId,
                    name: item.name,
                    artist: item.artist,
                    album: 'YouTube',
                    thumb: item.thumb,
                    videoId: item.videoId,
                    type: 'youtube'
                };
            } else { // Spotify ou outro
                track = {
                    id: item.id || `${item.name}_${item.artist || ''}`.replace(/\s/g, '_'),
                    name: item.name,
                    artist: item.artists?.map(a => a.name).join(', ') || item.artist || '',
                    album: item.album?.name || item.album || '',
                    thumb: item.album?.images?.[0]?.url || item.thumb || null,
                    preview: item.previewUrl || item.preview || null,
                    uri: item.uri,
                    type: 'spotify' // ou 'itunes'
                };
            }


            if (activeTab === 'playlists') {
                if (!currentPlaylist) currentPlaylist = { name: 'Nova Playlist', tracks: [] };
                currentPlaylist.tracks.push(track);
                updatePlaylistView();
                showNotification('Faixa adicionada à playlist.');
            }
        }

        // Inicia a reprodução de uma lista (álbum ou playlist)
        function startPlaybackFromList(tracks, startIndex) {
            playbackQueue.length = 0; // Limpa a fila atual
            playbackQueue.push(...tracks);
            queueIndex = startIndex;

            const trackToPlay = playbackQueue[queueIndex];
            if (trackToPlay.type === 'youtube') {
                playYouTubeVideo(trackToPlay.videoId, trackToPlay);
            } else if (trackToPlay.type === 'spotify') {
                playSpotifyTrack(trackToPlay);
            } else {
                playItunesPreview(trackToPlay);
            }
        }

        // Função para alternar entre tabs
        function switchTab(tabId) {
            document.querySelectorAll('.tab, nav a').forEach(el => {
                el.classList.remove('active');
            });
            document.querySelectorAll(`[data-tab="${tabId}"]`).forEach(el => {
                el.classList.add('active');
            });
            
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            document.getElementById(tabId).classList.add('active');
            // Se a aba de busca for selecionada, verifica se o login do Spotify foi feito
            if (tabId === 'search' && !spotifyAccessToken) {
                showNotification('Faça login com Spotify para buscar por artistas e álbuns.', 'info');
            }
        }

        // Load saved data on startup
        async function loadSavedData() {
            let playlists = [];
            if (currentUser && currentUser.uid) {
                const snapshot = await get(ref(database, `users/${currentUser.uid}/playlists`));
                if (snapshot.exists()) {
                    playlists = snapshot.val();
                }
            } else {
                playlists = JSON.parse(localStorage.getItem('playlists') || '[]');
            }

            renderPlaylistList(playlists);
            if (playlists.length > 0) {
                // Seleciona a primeira playlist por padrão
                const firstPlaylistItem = document.querySelector('.playlist-list-item');
                if (firstPlaylistItem) {
                    firstPlaylistItem.click();
                }
            } else {
                // Se não houver playlists, limpa o editor
                document.getElementById('btn-new-playlist').click();
            }
        }

        function renderPlaylistList(playlists) {
            if (!playlists) {
                playlists = JSON.parse(localStorage.getItem('playlists') || '[]');
            }
            const container = document.getElementById('playlist-list-container');
            container.innerHTML = playlists.map(p => `
                <div class="playlist-list-item" data-created-at="${p.createdAt}">
                    ${p.name}
                </div>
            `).join('');

            container.querySelectorAll('.playlist-list-item').forEach(item => {
                item.addEventListener('click', () => {
                    const createdAt = item.dataset.createdAt;
                    let allPlaylists = [];
                    if (currentUser && currentUser.uid) {
                        // Re-fetch from a local cache or state if available, for now re-read local.
                        allPlaylists = playlists;
                    } else {
                        allPlaylists = JSON.parse(localStorage.getItem('playlists') || '[]');
                    }
                    currentPlaylist = allPlaylists.find(p => p.createdAt === createdAt);
                    if (currentPlaylist) {
                        updatePlaylistView();
                    }

                    // Highlight active item
                    container.querySelectorAll('.playlist-list-item').forEach(el => el.classList.remove('active'));
                    item.classList.add('active');
                });
            });
        }

        async function addCurrentPlaylistToSpotify() {
            if (currentPlaylist && spotifyAccessToken) {
                const spotifyTracks = currentPlaylist.tracks.filter(t => t.type === 'spotify' && t.uri).map(t => t.uri);
                if (spotifyTracks.length > 0) await createSpotifyPlaylist(spotifyTracks);
                else showNotification('Nenhuma música do Spotify nesta playlist para adicionar.', 'info');
            }
        }

        // --- Inicialização e Event Listeners ---
        document.addEventListener('DOMContentLoaded', () => {
            initializeUI();
            // loadSavedData é chamado pelo onAuthStateChanged
            setupEventListeners();

            // Spotify login / player buttons
            // O botão de login agora está no modal do Firebase
            const spotifyLoginBtn = document.getElementById('spotify-login-btn');
            if (spotifyLoginBtn) spotifyLoginBtn.addEventListener('click', startSpotifyAuth);

            const spotifyPlayerBtn = document.querySelector('.btn-spotify-player');
            if (spotifyPlayerBtn) spotifyPlayerBtn.addEventListener('click', toggleSpotifyPlayer);
            
            setupLoginModal();
        });
        // Carrega as configurações do usuário ao iniciar
        document.addEventListener('DOMContentLoaded', () => {
            const localSettings = localStorage.getItem('user_settings');
            if (localSettings) {
                applySettings(JSON.parse(localSettings));
            }
        });

        // Funções de inicialização da UI
        function initializeUI() {
            // Funcionalidade de troca de abas
            document.querySelectorAll('.tab, nav a').forEach(element => {
                element.addEventListener('click', function() {
                    const tabId = this.getAttribute('data-tab');
                    if (!tabId) return;
                    switchTab(tabId);
                });
            });

            // Adicionar listener para o botão de adicionar à coleção nos resultados do YouTube
            document.querySelector('.search-results').addEventListener('click', function (e) {
                const button = e.target.closest('.add-to-collection-btn');
                if (button) {
                    const itemEl = button.closest('.search-result-item');
                    const trackData = { 
                        videoId: itemEl.dataset.videoId, 
                        name: itemEl.dataset.trackTitle, 
                        artist: itemEl.dataset.channelTitle, 
                        thumb: itemEl.dataset.thumbnailUrl,
                        type: 'youtube' // Essencial para a lógica da fila
                    };
                    addToPlaybackQueue(trackData); // Corrigido para adicionar à fila de reprodução
                    button.style.color = '#1DB954'; // Feedback visual
                }
            });
        }

        function clearPlaylistEditor() {
            currentPlaylist = null;
            document.querySelector('#playlist-name').value = '';
            document.querySelector('#playlist-description').value = '';
            document.querySelector('#playlists .track-list').innerHTML = `
                <h3 class="section-title"><i class="fas fa-music"></i> Músicas da Playlist</h3>
                <p style="text-align:center; color:#777;">Salve a playlist para adicionar músicas.</p>`;
            document.querySelectorAll('.playlist-list-item').forEach(el => el.classList.remove('active'));
            document.querySelector('#playlist-name').focus();
        }

        function setupEventListeners() {
            // Funcionalidade de busca
            const searchButton = document.querySelector('.btn-search');
            if (searchButton) {
                searchButton.addEventListener('click', searchMusic);
            } else {
                console.error('Botão de busca não encontrado!');
            }
            
            // Configurar tecla Enter no campo de busca
            document.querySelector('#search-input')?.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    e.preventDefault();
                    searchMusic();
                }
            });
            
            // Gerenciamento de playlist
            document.querySelector('.btn-save-playlist')?.addEventListener('click', savePlaylist);
            document.getElementById('btn-new-playlist').addEventListener('click', clearPlaylistEditor);
            document.getElementById('btn-add-to-spotify').addEventListener('click', () => {
                if (!currentPlaylist) showNotification('Selecione ou crie uma playlist primeiro.', 'error');
                else if (!spotifyAccessToken) showNotification('Faça login no Spotify para usar esta função.', 'error');
                else addCurrentPlaylistToSpotify();
            });
            
            // Controles do player (já configurados na função setupAudioPlayer)
            // Input para importação de playlist
            const importPlaylistInput = document.createElement('input');
            importPlaylistInput.type = 'file';
            importPlaylistInput.id = 'import-playlist-input';
            importPlaylistInput.accept = '.json';
            importPlaylistInput.style.display = 'none';
            importPlaylistInput.onchange = (e) => {
                importFromJson(e.target.files[0], (data) => {
                    if (currentUser) {
                        // TODO: Save imported playlist to Firebase
                    }
                    currentPlaylist = data;
                    updatePlaylistView();
                    showNotification('Playlist importada com sucesso!');
                });
            };
            document.body.appendChild(importPlaylistInput);
            // Logout button
            document.getElementById('spotify-logout-button')?.addEventListener('click', doLogout);
 
            // Personalization listeners
            if (!document.getElementById('btn-apply-customization')) {
                console.error('Botão de aplicar customização não encontrado');
                return;
            }
            document.getElementById('btn-apply-customization').addEventListener('click', () => {
                const settings = {
                    backgroundImage: document.getElementById('bg-image-url').value,
                    buttonSize: document.getElementById('button-size').value,
                    fontFamily: document.getElementById('font-select').value,
                    primaryColor: document.getElementById('primary-color-picker').value,
                    secondaryColor: document.getElementById('secondary-color-picker').value,
                    accentColor: document.getElementById('accent-color-picker').value,
                    borderRadius: document.getElementById('border-radius-slider').value,
                    darkMode: document.getElementById('dark-mode-toggle').checked
                };
                applySettings(settings);
                saveUserSettings(settings);
                showNotification('Aparência atualizada!');
            });

            document.getElementById('btn-reset-customization').addEventListener('click', () => {
                const defaultSettings = {
                    backgroundImage: '',
                    buttonSize: 1,
                    fontFamily: "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif",
                    primaryColor: '#6a11cb',
                    secondaryColor: '#2575fc',
                    accentColor: '#ff6b6b',
                    borderRadius: 12,
                    darkMode: false
                };
                applySettings(defaultSettings);
                saveUserSettings(defaultSettings);
                // Reset input fields
                document.getElementById('bg-image-url').value = '';
                document.getElementById('button-size').value = 1;
                document.getElementById('font-select').value = defaultSettings.fontFamily;
                document.getElementById('primary-color-picker').value = defaultSettings.primaryColor;
                document.getElementById('secondary-color-picker').value = defaultSettings.secondaryColor;
                document.getElementById('accent-color-picker').value = defaultSettings.accentColor;
                document.getElementById('border-radius-slider').value = defaultSettings.borderRadius;
                document.getElementById('dark-mode-toggle').checked = defaultSettings.darkMode;
                showNotification('Aparência resetada para o padrão.');
            });

            // Lyrics Modal Listeners
            const lyricsModal = document.getElementById('lyrics-modal');
            const lyricsBtn = document.getElementById('player-lyrics');
            const closeBtn = lyricsModal.querySelector('.close-btn');

            lyricsBtn.addEventListener('click', () => {
                if (currentPlayingTrackDetails) {
                    lyricsModal.style.display = 'block';
                    fetchAndShowLyrics(currentPlayingTrackDetails.artist, currentPlayingTrackDetails.name);
                }
            });

            closeBtn.addEventListener('click', () => {
                lyricsModal.style.display = 'none';
            });

            window.addEventListener('click', (event) => {
                if (event.target == lyricsModal) {
                    lyricsModal.style.display = 'none';
                }
            });

            // Equalizer Panel Toggle
            const eqBtn = document.getElementById('player-equalizer');
            const eqPanel = document.getElementById('equalizer-panel');
            eqBtn.addEventListener('click', () => {
                eqPanel.style.display = eqPanel.style.display === 'block' ? 'none' : 'block';
            });

            // Player UI Controls (Expand/Close)
            const expandBtn = document.getElementById('player-expand-toggle');
            const closePlayerBtn = document.getElementById('player-close');
            const playerDiv = document.getElementById('fixed-player');

            expandBtn.addEventListener('click', () => {
                playerDiv.classList.toggle('expanded');
                const icon = expandBtn.querySelector('i');
                icon.className = playerDiv.classList.contains('expanded') ? 'fas fa-chevron-down' : 'fas fa-chevron-up';
            });

            closeBtn.addEventListener('click', () => {
                playerDiv.classList.add('hidden');
                playerDiv.classList.remove('expanded'); // Garante que não reabra expandido
                expandBtn.querySelector('i').className = 'fas fa-chevron-up';
            });

            // Media Session API (para controles de mídia do dispositivo)
            if ('mediaSession' in navigator) {
                // Define os manipuladores de ação uma única vez
                navigator.mediaSession.setActionHandler('play', () => togglePlay());
                navigator.mediaSession.setActionHandler('pause', () => togglePlay());
                navigator.mediaSession.setActionHandler('nexttrack', () => playNext());
                navigator.mediaSession.setActionHandler('previoustrack', () => playPrevious());
                navigator.mediaSession.setActionHandler('seekbackward', (details) => {
                    handleSeek(-(details.seekOffset || 10)); // Retrocede 10 segundos por padrão
                });
                navigator.mediaSession.setActionHandler('seekforward', (details) => {
                    handleSeek(details.seekOffset || 10); // Avança 10 segundos por padrão
                });
                // Outros manipuladores como 'stop', 'seekto' podem ser adicionados
            }
        }
        
        // Expor funções para o escopo global para que os `onclick` no HTML funcionem
        window.exportToJson = exportToJson;


        // --- Funções de Personalização ---
        function applySettings(settings) {
            if (settings.backgroundImage) {
                document.body.style.backgroundImage = `url('${settings.backgroundImage}')`;
            } else {
                document.body.style.backgroundImage = 'linear-gradient(135deg, var(--light-color) 0%, #e0eafc 100%)';
            }
            document.documentElement.style.setProperty('--button-scale', settings.buttonSize || 1);
            document.body.style.fontFamily = settings.fontFamily || "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif";
            document.documentElement.style.setProperty('--primary-color', settings.primaryColor || '#6a11cb');
            document.documentElement.style.setProperty('--secondary-color', settings.secondaryColor || '#2575fc');
            document.documentElement.style.setProperty('--accent-color', settings.accentColor || '#ff6b6b');
            document.documentElement.style.setProperty('--border-radius', (settings.borderRadius || 12) + 'px');

            if (settings.darkMode) {
                document.body.classList.add('dark-mode');
            } else { document.body.classList.remove('dark-mode'); }
        }

        async function saveUserSettings(settings) {
            if (currentUser && currentUser.uid) {
                // Salva no Firebase se o usuário estiver logado
                const userSettingsRef = ref(database, 'users/' + currentUser.uid + '/settings');
                await set(userSettingsRef, settings);
            } else {
                // Salva localmente se não estiver logado
                localStorage.setItem('user_settings', JSON.stringify(settings));
            }
        }

        async function loadUserSettings(userId) {
            const userSettingsRef = ref(database, `users/${userId}/settings`);
            const snapshot = await get(userSettingsRef);
            if (snapshot.exists()) {
                const settings = snapshot.val();
                applySettings(settings);
                // Atualiza os controles na UI
                document.getElementById('bg-image-url').value = settings.backgroundImage || '';
                document.getElementById('button-size').value = settings.buttonSize || 1;
                document.getElementById('font-select').value = settings.fontFamily || "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif";
                document.getElementById('primary-color-picker').value = settings.primaryColor || '#6a11cb';
                document.getElementById('secondary-color-picker').value = settings.secondaryColor || '#2575fc';
                document.getElementById('accent-color-picker').value = settings.accentColor || '#ff6b6b';
                document.getElementById('border-radius-slider').value = settings.borderRadius || 12;
                document.getElementById('dark-mode-toggle').checked = settings.darkMode || false;

            }
        }

        // --- Funções de Letras (Lyrics) ---
        async function fetchAndShowLyrics(artist, title) {
            const lyricsContainer = document.getElementById('lyrics-text-container');
            const lyricsTitle = document.getElementById('lyrics-title');
            lyricsTitle.textContent = `Letra de "${title}"`;
            lyricsContainer.innerHTML = '<p>Buscando letra...</p>';

            try {
                const response = await fetch(`https://api.lyrics.ovh/v1/${encodeURIComponent(artist)}/${encodeURIComponent(title)}`);
                if (!response.ok) {
                    throw new Error('Letra não encontrada.');
                }
                const data = await response.json();
                lyricsContainer.textContent = data.lyrics || 'Letra não disponível para esta música.';
            } catch (error) {
                console.error('Erro ao buscar letra:', error);
                lyricsContainer.textContent = 'Não foi possível encontrar a letra para esta música.';
            }
        }

        // --- Funções do Equalizador (Web Audio API) ---
        let audioContext;
        let sourceNode;
        let gainNode;
        let filters = [];
        const FREQUENCY_BANDS = [60, 170, 310, 600, 1000, 3000, 6000, 12000, 14000, 16000];

        function setupAudioNodes() {
            if (audioContext) return; // Já inicializado

            audioContext = new (window.AudioContext || window.webkitAudioContext)();
            sourceNode = audioContext.createMediaElementSource(audioPlayer); // Conecta ao player de áudio do iTunes
            gainNode = audioContext.createGain();

            // Cria um filtro para cada banda
            filters = FREQUENCY_BANDS.map(freq => {
                const filter = audioContext.createBiquadFilter();
                filter.type = 'peaking';
                filter.frequency.value = freq;
                filter.Q.value = 1;
                filter.gain.value = 0; // Inicia com ganho 0 dB
                return filter;
            });

            // Conecta os nós em cadeia: source -> filter1 -> filter2 -> ... -> gain -> destination
            sourceNode.connect(filters[0]);
            for (let i = 0; i < filters.length - 1; i++) {
                filters[i].connect(filters[i + 1]);
            }
            filters[filters.length - 1].connect(gainNode);
            gainNode.connect(audioContext.destination);

            // Gera os sliders na UI
            const bandsContainer = document.getElementById('equalizer-bands');
            bandsContainer.innerHTML = FREQUENCY_BANDS.map((freq, i) => `
                <div class="band">
                    <input type="range" id="band-${i}" min="-15" max="15" step="1" value="0">
                    <label>${freq < 1000 ? freq + 'Hz' : (freq / 1000) + 'k'}</label>
                </div>
            `).join('');

            // Adiciona listeners aos sliders
            filters.forEach((filter, i) => {
                document.getElementById(`band-${i}`).addEventListener('input', (e) => {
                    filter.gain.value = e.target.value;
                });
            });

            // Listeners para presets
            document.querySelector('.equalizer-presets').addEventListener('click', (e) => {
                if (e.target.tagName === 'BUTTON' && e.target.dataset.preset) {
                    applyEQPreset(e.target.dataset.preset);
                }
            });
            document.getElementById('eq-reset-btn').addEventListener('click', () => applyEQPreset('flat'));
        }

        // Função para conectar o player do YouTube ao AudioContext (se possível)
        // Esta é uma abordagem experimental e pode não funcionar em todos os navegadores.
        // A API do YouTube não oferece um jeito oficial de acessar o stream de áudio.

        // Configurar controles do player local (para previews do iTunes)
        audioPlayer.addEventListener('timeupdate', () => {
            if (currentPlayingMediaType === 'itunes') {
                const duration = isFinite(audioPlayer.duration) ? audioPlayer.duration : 0;
                const progress = duration ? (audioPlayer.currentTime / duration) * 100 : 0;
                document.getElementById('player-progress').style.width = `${progress}%`;
                document.getElementById('player-current-time').textContent = formatTime(audioPlayer.currentTime);
                document.getElementById('player-duration').textContent = formatTime(duration);
            }
        });
        audioPlayer.addEventListener('ended', () => { if (currentPlayingMediaType === 'itunes') playNext(); });
        audioPlayer.addEventListener('play', () => { if (currentPlayingMediaType === 'itunes') document.getElementById('player-toggle-play').querySelector('i').className = 'fas fa-pause'; });
        audioPlayer.addEventListener('pause', () => { if (currentPlayingMediaType === 'itunes') document.getElementById('player-toggle-play').querySelector('i').className = 'fas fa-play'; });

        document.getElementById('player-toggle-play').addEventListener('click', togglePlay);
        document.getElementById('player-next').addEventListener('click', playNext);
        document.getElementById('player-previous').addEventListener('click', playPrevious);
        document.getElementById('player-progress-bar').addEventListener('click', (e) => {
            const pos = (e.clientX - e.target.getBoundingClientRect().left) / e.target.offsetWidth;
            seekInActivePlayer(pos);
        });

        // Inicializa o AudioContext quando o usuário interage pela primeira vez
        document.body.addEventListener('click', () => {
            if (!audioContext) {
                setupAudioNodes();
            }
        }, { once: true });

        function applyEQPreset(preset) {
            const presets = {
                flat: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                pop: [2, 4, 5, 2, -1, -2, 0, 1, 2, 3],
                rock: [5, 3, -2, -3, -1, 2, 4, 5, 6, 7],
                jazz: [4, 2, 1, 2, -2, -2, 0, 1, 3, 4]
            };
            const values = presets[preset];
            if (values) {
                filters.forEach((filter, i) => {
                    const gain = values[i];
                    filter.gain.value = gain;
                    document.getElementById(`band-${i}`).value = gain;
                });
            }
        }

        // Expor funções globais
        window.onYouTubeIframeAPIReady = onYouTubeIframeAPIReady;

    </script>    
    <script>
        // Função para alternar abas, agora no escopo global
        function switchTab(tabId) {
            // Remove a classe 'active' de todos os links de navegação e abas visuais
            document.querySelectorAll('nav a, .tab').forEach(el => el.classList.remove('active'));
            
            // Adiciona a classe 'active' aos elementos da nova aba
            document.querySelectorAll(`[data-tab="${tabId}"]`).forEach(el => el.classList.add('active'));

            // Esconde todos os conteúdos de aba e exibe o correto
            document.querySelectorAll('.tab-content').forEach(content => {
                content.style.display = content.id === tabId ? 'block' : 'none';
            });
        }
    </script>
    <script>
        // Lógica para adicionar abas dinamicamente
        document.addEventListener('DOMContentLoaded', function() {
            const btnAddPlaylists = document.getElementById('btn-add-tab-playlists');
            const btnAddSearch = document.getElementById('btn-add-tab-search');
            const btnAddCommunity = document.getElementById('btn-add-tab-community');

            btnAddPlaylists.addEventListener('click', function() {
                document.getElementById('nav-item-playlists').style.display = 'list-item';
                document.getElementById('main-tab-playlists').style.display = 'block';
                switchTab('playlists'); // Ativa a aba recém-adicionada
                this.disabled = true;
                this.textContent = 'Aba "Editor" Adicionada';
                this.style.backgroundColor = '#e9ecef';
                this.style.color = '#6c757d';
                this.style.cursor = 'not-allowed';
            });

            btnAddSearch.addEventListener('click', function() {
                document.getElementById('nav-item-search').style.display = 'list-item';
                document.getElementById('main-tab-search').style.display = 'block';
                switchTab('search'); // Ativa a aba recém-adicionada
                this.disabled = true;
                this.textContent = 'Aba "Busca" Adicionada';
                this.style.backgroundColor = '#e9ecef';
                this.style.color = '#6c757d';
                this.style.cursor = 'not-allowed';
                generateShareableLink(); // Atualiza o link
            });

            btnAddCommunity.addEventListener('click', function() {
                document.getElementById('nav-item-community').style.display = 'list-item';
                document.getElementById('main-tab-community').style.display = 'block';
                switchTab('community'); // Ativa a aba recém-adicionada
                this.disabled = true;
                this.textContent = 'Aba "Comunidade" Adicionada';
                this.style.backgroundColor = '#e9ecef';
                this.style.color = '#6c757d';
                this.style.cursor = 'not-allowed';
                generateShareableLink(); // Atualiza o link
            });

            // --- Lógica de Compartilhamento e Carregamento de URL ---

            const shareableLinkInput = document.getElementById('shareable-link-input');
            const copyLinkBtn = document.getElementById('btn-copy-link');
            const saveAndShareBtn = document.getElementById('btn-save-and-share');
            const customSlugInput = document.getElementById('custom-link-slug');
            const linkStatusMsg = document.getElementById('link-status-message');

            function generateShareableLink() {
                const baseUrl = window.location.origin + window.location.pathname;
                const params = new URLSearchParams();
                
                const settings = {
                    bg: document.getElementById('bg-image-url').value,
                    btnSize: document.getElementById('button-size').value,
                    font: document.getElementById('font-select').value,
                    c1: document.getElementById('primary-color-picker').value,
                    c2: document.getElementById('secondary-color-picker').value,
                    c3: document.getElementById('accent-color-picker').value,
                    radius: document.getElementById('border-radius-slider').value,
                    dark: document.getElementById('dark-mode-toggle').checked,
                    tabs: []
                };

                if (document.getElementById('nav-item-playlists').style.display !== 'none') settings.tabs.push('playlists');
                if (document.getElementById('nav-item-search').style.display !== 'none') settings.tabs.push('search');
                if (document.getElementById('nav-item-community').style.display !== 'none') settings.tabs.push('community');

                // Adiciona parâmetros apenas se não forem os valores padrão
                if (settings.bg) params.set('bg', settings.bg);
                if (settings.btnSize !== '1') params.set('btnSize', settings.btnSize);
                if (settings.font !== "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif") params.set('font', settings.font);
                if (settings.c1 !== '#6a11cb') params.set('c1', settings.c1);
                if (settings.c2 !== '#2575fc') params.set('c2', settings.c2);
                if (settings.c3 !== '#ff6b6b') params.set('c3', settings.c3);
                if (settings.radius !== '12') params.set('radius', settings.radius);
                if (settings.dark) params.set('dark', '1');
                if (settings.tabs.length > 0) params.set('tabs', settings.tabs.join(','));

                const paramString = params.toString();
                shareableLinkInput.value = paramString ? `${baseUrl}?${paramString}` : baseUrl;
            }

            function applySettingsFromURL() {
                const params = new URLSearchParams(window.location.search);
                const themeSlug = params.get('theme');
                const settings = {
                    bg: params.get('bg'),
                    btnSize: params.get('btnSize'),
                    font: params.get('font'),
                    c1: params.get('c1'),
                    c2: params.get('c2'),
                    c3: params.get('c3'),
                    radius: params.get('radius'),
                    dark: params.get('dark') === '1',
                    tabs: params.get('tabs')?.split(',') || []
                };

                if (themeSlug) {
                    // Prioriza carregar o tema pelo slug
                    const themeRef = ref(database, `public_themes/${themeSlug}`);
                    get(themeRef).then(snapshot => {
                        if (snapshot.exists()) {
                            const themeSettings = snapshot.val();
                            applySettings(themeSettings); // Função auxiliar para aplicar as configurações
                            showNotification(`Tema "${themeSlug}" carregado!`, 'info');
                        } else {
                            showNotification(`Tema "${themeSlug}" não encontrado.`, 'error');
                        }
                    }).catch(error => {
                        console.error("Erro ao carregar tema:", error);
                        showNotification('Erro ao carregar o tema.', 'error');
                    });
                    return; // Para a execução para não aplicar os parâmetros antigos
                }

                // Aplica as configurações e atualiza os controles da UI
                if (settings.bg) { document.getElementById('bg-image-url').value = settings.bg; }
                if (settings.btnSize) { document.getElementById('button-size').value = settings.btnSize; }
                if (settings.font) { document.getElementById('font-select').value = settings.font; }
                if (settings.c1) { document.getElementById('primary-color-picker').value = settings.c1; }
                if (settings.c2) { document.getElementById('secondary-color-picker').value = settings.c2; }
                if (settings.c3) { document.getElementById('accent-color-picker').value = settings.c3; }
                if (settings.radius) { document.getElementById('border-radius-slider').value = settings.radius; }
                if (settings.dark) { document.getElementById('dark-mode-toggle').checked = true; }
                
                // Simula o clique no botão de aplicar para que as configurações visuais sejam carregadas
                document.getElementById('btn-apply-customization').click();

                // Adiciona as abas
                if (settings.tabs.includes('playlists')) btnAddPlaylists.click();
                if (settings.tabs.includes('search')) btnAddSearch.click();
                if (settings.tabs.includes('community')) btnAddCommunity.click();

                // Gera o link compartilhável com base nos parâmetros carregados
                generateShareableLink();
            }

            // Função auxiliar para aplicar configurações de um objeto
            function applySettings(settingsObj) {
                if (!settingsObj) return;
                document.getElementById('bg-image-url').value = settingsObj.bg || '';
                document.getElementById('button-size').value = settingsObj.btnSize || '1';
                document.getElementById('font-select').value = settingsObj.font || "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif";
                document.getElementById('primary-color-picker').value = settingsObj.c1 || '#6a11cb';
                document.getElementById('secondary-color-picker').value = settingsObj.c2 || '#2575fc';
                document.getElementById('accent-color-picker').value = settingsObj.c3 || '#ff6b6b';
                document.getElementById('border-radius-slider').value = settingsObj.radius || '12';
                document.getElementById('dark-mode-toggle').checked = settingsObj.dark || false;
                
                document.getElementById('btn-apply-customization').click();

                // Adiciona as abas
                const tabs = settingsObj.tabs || [];
                if (tabs.includes('playlists')) btnAddPlaylists.click();
                if (tabs.includes('search')) btnAddSearch.click();
                if (tabs.includes('community')) btnAddCommunity.click();
                generateShareableLink();
            }

            // Gera o link ao carregar e sempre que uma personalização é aplicada
            document.getElementById('btn-apply-customization').addEventListener('click', generateShareableLink);
            document.getElementById('btn-reset-customization').addEventListener('click', () => setTimeout(generateShareableLink, 50));

            copyLinkBtn.addEventListener('click', function() {
                shareableLinkInput.select();
                document.execCommand('copy');
                this.innerHTML = '<i class="fas fa-check"></i> Copiado!';
                setTimeout(() => { this.innerHTML = '<i class="fas fa-copy"></i> Copiar'; }, 2000);
            });

            saveAndShareBtn.addEventListener('click', async () => {
                const slug = customSlugInput.value.trim().toLowerCase().replace(/[^a-z0-9-]/g, '');
                if (!slug) {
                    linkStatusMsg.textContent = 'Por favor, digite um nome para o seu link.';
                    linkStatusMsg.style.color = 'var(--accent-color)';
                    return;
                }

                const themeRef = ref(database, `public_themes/${slug}`);
                const snapshot = await get(themeRef);

                if (snapshot.exists()) {
                    linkStatusMsg.textContent = 'Este nome já está em uso. Tente outro.';
                    linkStatusMsg.style.color = 'var(--accent-color)';
                } else {
                    // Nome está livre, salva as configurações
                    const settings = {
                        bg: document.getElementById('bg-image-url').value,
                        btnSize: document.getElementById('button-size').value,
                        font: document.getElementById('font-select').value,
                        c1: document.getElementById('primary-color-picker').value,
                        c2: document.getElementById('secondary-color-picker').value,
                        c3: document.getElementById('accent-color-picker').value,
                        radius: document.getElementById('border-radius-slider').value,
                        dark: document.getElementById('dark-mode-toggle').checked,
                        tabs: []
                    };
                    if (document.getElementById('nav-item-playlists').style.display !== 'none') settings.tabs.push('playlists');
                    if (document.getElementById('nav-item-search').style.display !== 'none') settings.tabs.push('search');
                    if (document.getElementById('nav-item-community').style.display !== 'none') settings.tabs.push('community');

                    await set(themeRef, settings);

                    const baseUrl = window.location.origin + window.location.pathname;
                    shareableLinkInput.value = `${baseUrl}?theme=${slug}`;
                    
                    linkStatusMsg.textContent = 'Link salvo e gerado com sucesso!';
                    linkStatusMsg.style.color = 'green';
                    setTimeout(() => { linkStatusMsg.textContent = ''; }, 4000);
                }
            });


            // --- Lógica do Feed da Comunidade ---
            const postModal = document.getElementById('post-feed-modal');
            const btnShareMySite = document.getElementById('btn-share-my-site');
            const btnConfirmPost = document.getElementById('btn-confirm-post');
            const btnCancelPost = document.getElementById('btn-cancel-post');
            const closePostModal = document.getElementById('close-post-modal');

            btnShareMySite.addEventListener('click', () => {
                if (!window.currentUser) {
                    showNotification('Você precisa estar logado para postar no feed.', 'error');
                    // Abre o modal de login se não estiver logado
                    document.getElementById('firebase-login-modal').style.display = 'block';
                    return;
                }
                postModal.style.display = 'flex';
            });

            closePostModal.onclick = () => postModal.style.display = 'none';
            btnCancelPost.onclick = () => postModal.style.display = 'none';
            window.onclick = (e) => { if (e.target == postModal) postModal.style.display = 'none'; };

            btnConfirmPost.addEventListener('click', async () => {
                const themeName = document.getElementById('theme-name-input').value.trim();
                if (!themeName) {
                    document.getElementById('post-error-message').textContent = 'Por favor, dê um nome ao seu tema.';
                    return;
                }

                const postData = {
                    authorName: currentUser.name || 'Anônimo',
                    authorId: window.currentUser?.uid,
                    themeName: themeName,
                    shareLink: document.getElementById('shareable-link-input').value,
                    timestamp: new Date().toISOString()
                };

                try {
                    const feedRef = ref(database, 'feedPosts/' + Date.now()); // Usa timestamp como ID único
                    await set(feedRef, postData);
                    showNotification('Seu tema foi publicado no feed!', 'info');
                    postModal.style.display = 'none';
                    document.getElementById('theme-name-input').value = '';
                    document.getElementById('post-error-message').textContent = '';
                    loadCommunityFeed(); // Recarrega o feed
                } catch (error) {
                    console.error("Erro ao postar no feed:", error);
                    showNotification('Ocorreu um erro ao publicar. Tente novamente.', 'error');
                }
            });

            function applyThemeFromLink(link) {
                // Simplesmente redireciona a página para a URL com os parâmetros do tema
                // A lógica `applySettingsFromURL` cuidará do resto ao carregar a página.
                if (link && link.includes('?')) {
                    window.location.href = link;
                } else {
                    showNotification('Link de tema inválido.', 'error');
                }
            }


            // Adiciona os listeners para a troca de abas existentes
            document.querySelectorAll('.tab, nav a').forEach(element => {
                element.addEventListener('click', function(e) {
                    e.preventDefault(); // Previne o comportamento padrão do link
                    const tabId = this.getAttribute('data-tab');
                    if (tabId) {
                        switchTab(tabId);
                    }
                });
            });

            // Aplica configurações da URL ao carregar a página
            applySettingsFromURL();
            loadCommunityFeed();
            generateShareableLink(); // Gera o link inicial

            async function loadCommunityFeed() {
                const feedContainer = document.getElementById('community-feed-container');
                feedContainer.innerHTML = '<p>Carregando feed...</p>';
                try {
                    const feedRef = ref(database, 'feedPosts');
                    const snapshot = await get(feedRef);
                    if (snapshot.exists()) {
                        const posts = snapshot.val();
                        const postKeys = Object.keys(posts).sort((a, b) => b - a); // Ordena do mais novo para o mais antigo
                        
                        feedContainer.innerHTML = ''; // Limpa o container
                        postKeys.forEach(key => {
                            const post = posts[key];
                            const postCard = document.createElement('div');
                            postCard.className = 'feed-post-card';
                            postCard.innerHTML = `
                                <div class="feed-post-content">
                                    <h4 class="feed-post-title">${post.themeName}</h4>
                                    <p class="feed-post-author">por ${post.authorName}</p>
                                    <button class="btn btn-sm">Aplicar Tema</button>
                                </div>
                            `;
                            postCard.querySelector('button').addEventListener('click', () => applyThemeFromLink(post.shareLink));
                            feedContainer.appendChild(postCard);
                        });
                    } else {
                        feedContainer.innerHTML = '<p style="text-align: center; color: #777;">Nenhuma publicação no feed ainda. Seja o primeiro a postar!</p>';
                    }
                } catch (error) {
                    console.error("Erro ao carregar o feed:", error);
                    feedContainer.innerHTML = '<p style="color: var(--accent-color);">Não foi possível carregar o feed.</p>';
                }
            }
        });
    </script>
</body>
</html>
