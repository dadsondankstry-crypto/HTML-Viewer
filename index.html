<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>HTML Viewer</title>
    <link rel="manifest" href="manifest.json">
    <style>
        body { font-family: sans-serif; margin: 0; background: #f0f2f5; height: 100vh; overflow: hidden; display: flex; flex-direction: column; }
        
        /* Cabeçalho e Menu */
        .header { padding: 20px; text-align: center; background: #fff; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .menu { padding: 15px; display: flex; flex-direction: column; gap: 12px; flex-grow: 1; }
        
        /* Itens de Configuração com Switch */
        .config-box { 
            display: flex; justify-content: space-between; align-items: center; 
            background: #fff; padding: 15px; border-radius: 12px; 
        }

        /* Botões Principais */
        .btn { 
            padding: 16px; font-size: 16px; font-weight: bold; border: none; 
            border-radius: 12px; cursor: pointer; color: white; width: 100%;
        }
        .btn-colar { background: #28a745; margin-top: 10px; }
        .btn-salvar { background: #007bff; }
        .btn-share { background: #6f42c1; }

        /* Viewer e Iframe */
        #viewer-container { 
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: white; display: none; z-index: 998; 
        }
        iframe { width: 100%; height: 100%; border: none; }

        /* Botão Flutuante (Arrastável) */
        #dragBtn { 
            position: fixed; bottom: 30px; right: 30px; 
            width: 60px; height: 60px; background: #007bff; 
            color: white; border-radius: 50%; display: flex; 
            align-items: center; justify-content: center; font-size: 24px;
            z-index: 1000; box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            touch-action: none; 
        }

        /* Botão de fechar (X) */
        #btnClose { 
            position: fixed; top: 15px; right: 15px; width: 40px; height: 40px; 
            background: rgba(255,0,0,0.7); color: white; border: none; 
            border-radius: 50%; z-index: 1001; display: none; font-weight: bold;
        }

        /* Toast (Avisos) */
        #toast { 
            position: fixed; bottom: 110px; left: 50%; transform: translateX(-50%);
            background: rgba(0,0,0,0.8); color: white; padding: 10px 20px; 
            border-radius: 25px; display: none; z-index: 1002; font-size: 14px;
        }

        textarea { display: none; } /* Oculto conforme pedido */
    </style>
</head>
<body>

<div class="header">
    <h2 style="margin:0;">HTML Viewer</h2>
</div>

<div class="menu">
    <div class="config-box">
        <span>Auto-Abrir ao Colar</span>
        <input type="checkbox" id="autoAbrir">
    </div>
    <div class="config-box">
        <span>Ocultar botão X (Gesto)</span>
        <input type="checkbox" id="modoGesto">
    </div>

    <button class="btn btn-colar" onclick="acaoColar()">COLAR CÓDIGO</button>
    <button class="btn btn-salvar" onclick="exportar('salvar')">SALVAR ARQUIVO</button>
    <button class="btn btn-share" onclick="exportar('compartilhar')">COMPARTILHAR</button>
</div>

<textarea id="dbConteudo"></textarea>
<div id="viewer-container"><iframe id="meuIframe"></iframe></div>
<button id="btnClose" onclick="fecharJanela()">X</button>
<div id="dragBtn">👁️</div>
<div id="toast"></div>

<script>
    const db = document.getElementById('dbConteudo');
    const iframe = document.getElementById('meuIframe');
    const container = document.getElementById('viewer-container');
    const btnX = document.getElementById('btnClose');
    const dragBtn = document.getElementById('dragBtn');
    const toast = document.getElementById('toast');
    
    // Configurações e Memória
    const optAuto = document.getElementById('autoAbrir');
    const optGesto = document.getElementById('modoGesto');

    optAuto.checked = localStorage.getItem('cfg_auto') === 'true';
    optGesto.checked = localStorage.getItem('cfg_gesto') === 'true';

    optAuto.onchange = () => localStorage.setItem('cfg_auto', optAuto.checked);
    optGesto.onchange = () => localStorage.setItem('cfg_gesto', optGesto.checked);

    function notify(msg) {
        toast.innerText = msg;
        toast.style.display = 'block';
        setTimeout(() => toast.style.display = 'none', 2000);
    }

    async function acaoColar() {
        try {
            const texto = await navigator.clipboard.readText();
            db.value = texto;
            notify("Conteúdo Colado!");
            if(optAuto.checked) abrirJanela();
        } catch (e) { notify("Erro ao acessar área de transferência."); }
    }

    function abrirJanela() {
        if(!db.value) return notify("Nada para exibir.");
        iframe.srcdoc = db.value;
        container.style.display = 'block';
        if(!optGesto.checked) btnX.style.display = 'block';
    }

    function fecharJanela() {
        container.style.display = 'none';
        btnX.style.display = 'none';
    }

    // Lógica do Botão Móvel (Segurar para mover)
    let holdTimer;
    let movendo = false;

    dragBtn.addEventListener('touchstart', (e) => {
        holdTimer = setTimeout(() => { movendo = true; notify("Modo mover ativado"); }, 600);
    });

    dragBtn.addEventListener('touchmove', (e) => {
        if(!movendo) return;
        let t = e.touches[0];
        dragBtn.style.left = (t.clientX - 30) + 'px';
        dragBtn.style.top = (t.clientY - 30) + 'px';
    });

    dragBtn.addEventListener('touchend', () => {
        clearTimeout(holdTimer);
        if(!movendo) abrirJanela();
        movendo = false;
    });

    function exportar(tipo) {
        const codigo = db.value;
        if(!codigo) return notify("Vazio!");

        // Busca <title> no código
        const match = codigo.match(/<title>(.*?)<\/title>/i);
        const nomeArquivo = (match && match[1]) ? match[1].trim() : "arquivo_1";
        
        const blob = new Blob([codigo], { type: 'text/html' });

        if(tipo === 'salvar') {
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `${nomeArquivo}.html`;
            link.click();
            notify("Download iniciado!");
        } else {
            const f = new File([blob], `${nomeArquivo}.html`, { type: 'text/html' });
            if(navigator.share) {
                navigator.share({ files: [f], title: nomeArquivo }).catch(() => {});
            } else { notify("Compartilhamento não suportado."); }
        }
    }

    if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('sw.js');
    }
</script>
</body>
</html>