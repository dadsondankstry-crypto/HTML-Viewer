<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>HTML Viewer</title>
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#007bff">
    <link rel="apple-touch-icon" href="icon-192.png">
    <style>
        :root { --primary: #007bff; --success: #28a745; --bg: #f0f2f5; }
        body { font-family: sans-serif; margin: 0; background: var(--bg); height: 100vh; overflow: hidden; }
        
        .container { padding: 20px; display: flex; flex-direction: column; gap: 15px; }
        h2 { text-align: center; color: #333; margin-bottom: 20px; }
        
        .config-btn { 
            padding: 15px; border-radius: 12px; border: 2px solid #ccc;
            background: white; cursor: pointer; font-weight: bold;
            display: flex; justify-content: center; align-items: center;
            transition: all 0.3s ease;
            text-align: center;
        }
        .config-btn.active { 
            border-color: var(--success); 
            color: var(--success); 
            background: #eaffea; 
            box-shadow: 0 0 8px rgba(40, 167, 69, 0.2);
        }

        .btn-acao { 
            padding: 18px; font-size: 16px; font-weight: bold; border: none; 
            border-radius: 12px; cursor: pointer; color: white; transition: opacity 0.2s;
        }
        .btn-colar { background: var(--success); }
        .btn-salvar { background: var(--primary); }
        .btn-share { background: #6f42c1; }
        .btn-acao:active { opacity: 0.8; }

        #viewer-container { 
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: white; display: none; z-index: 998; 
        }
        iframe { width: 100%; height: 100%; border: none; }

        #toast { 
            position: fixed; bottom: 50px; left: 50%; transform: translateX(-50%);
            background: rgba(0,0,0,0.85); color: white; padding: 12px 24px; 
            border-radius: 30px; display: none; z-index: 1002; font-size: 14px; text-align: center;
        }

        textarea { display: none; }
    </style>
</head>
<body>

<div class="container">
    <h2>HTML Viewer</h2>
    
    <div id="btnAuto" class="config-btn" onclick="toggleCfg('auto')">
        Auto-Abrir ao Colar
    </div>

    <button class="btn-acao btn-colar" onclick="executarColar()">COLAR CÓDIGO</button>
    <button class="btn-acao btn-salvar" onclick="gerarArquivo('salvar')">SALVAR .HTML</button>
    <button class="btn-acao btn-share" onclick="gerarArquivo('share')">COMPARTILHAR</button>
</div>

<textarea id="hiddenStorage"></textarea>
<div id="viewer-container"><iframe id="contentFrame"></iframe></div>
<div id="toast"></div>

<script>
    const storage = document.getElementById('hiddenStorage');
    const frame = document.getElementById('contentFrame');
    const vContainer = document.getElementById('viewer-container');
    
    let cfg = {
        auto: localStorage.getItem('cfg_auto') === 'true'
    };

    function atualizarInterface() {
        document.getElementById('btnAuto').className = cfg.auto ? 'config-btn active' : 'config-btn';
    }
    atualizarInterface();

    function toggleCfg(tipo) {
        cfg[tipo] = !cfg[tipo];
        localStorage.setItem('cfg_'+tipo, cfg[tipo]);
        atualizarInterface();
        avisar(cfg[tipo] ? "Ativado" : "Desativado");
    }

    function avisar(msg) {
        const t = document.getElementById('toast');
        t.innerText = msg; t.style.display = 'block';
        setTimeout(() => t.style.display = 'none', 2000);
    }

    async function executarColar() {
        try {
            const txt = await navigator.clipboard.readText();
            storage.value = txt;
            avisar("Conteúdo Colado!");
            if(cfg.auto) alternarVisualizacao(true);
        } catch(e) { avisar("Erro ao ler área de transferência."); }
    }

    function alternarVisualizacao(forcarAbrir = null) {
        const estaAberto = vContainer.style.display === 'block';
        const novoEstado = forcarAbrir !== null ? forcarAbrir : !estaAberto;

        if(novoEstado) {
            if(!storage.value) return avisar("Cole um código primeiro!");
            frame.srcdoc = storage.value;
            vContainer.style.display = 'block';
            history.pushState({viewer: true}, "Viewer", "#viewer");
        } else {
            vContainer.style.display = 'none';
            frame.srcdoc = ""; 
            if(history.state && history.state.viewer) history.back();
        }
    }

    window.addEventListener('keydown', (e) => { 
        if(e.key === "Escape" && vContainer.style.display === 'block') {
            alternarVisualizacao(false);
        }
    });

    window.addEventListener('popstate', (e) => {
        if(vContainer.style.display === 'block') {
            vContainer.style.display = 'none';
            frame.srcdoc = "";
        }
    });

    function gerarArquivo(modo) {
        const cod = storage.value;
        if(!cod) return avisar("Sem conteúdo!");
        
        const buscaTitulo = cod.match(/<title>(.*?)<\/title>/i);
        const nomeFinal = buscaTitulo ? buscaTitulo[1].trim() : "arquivo_html";
        
        const blob = new Blob([cod], {type:'text/html'});

        if(modo === 'salvar') {
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = nomeFinal + ".html";
            link.click();
            avisar("Arquivo baixado!");
        } else {
            const file = new File([blob], nomeFinal + ".html", {type:'text/html'});
            if(navigator.share) {
                navigator.share({files:[file], title: nomeFinal});
            } else { avisar("Erro no compartilhamento."); }
        }
    }

    // Registro do Service Worker
    if ('serviceWorker' in navigator) {
        window.addEventListener('load', () => {
            navigator.serviceWorker.register('sw.js')
                .then(reg => console.log('SW registrado', reg))
                .catch(err => console.log('Erro no SW', err));
        });
    }
</script>
</body>
</html>