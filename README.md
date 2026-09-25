<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>10º Batalhão de Caçadores - Sistema de Efetivo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body class="bg-[#0f172a] text-slate-100 font-sans antialiased min-h-screen">

    <div class="max-w-7xl mx-auto p-4 md:p-8">
        
        <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 border-b border-slate-800 pb-6 gap-4">
            <div>
                <h1 class="text-3xl font-black text-white tracking-tight flex items-center gap-2">
                    <span class="text-indigo-500">🛡️</span> 10º Batalhão de Caçadores
                </h1>
                <p class="text-sm text-slate-400 mt-1">Análise para distribuição de efetivo</p>
            </div>
            
            <div class="bg-slate-900 p-3 rounded-xl border border-slate-800 flex items-center gap-3">
                <label class="text-xs font-bold uppercase tracking-wider text-slate-400">Atualizar:</label>
                <input type="file" id="csv_file" accept=".csv" class="text-xs text-slate-400
                    file:mr-2 file:py-1 file:px-3
                    file:rounded file:border-0
                    file:text-xs file:font-bold
                    file:bg-indigo-600 file:text-white
                    hover:file:bg-indigo-700 cursor-pointer" />
            </div>
        </div>

        <div class="mb-6 bg-gradient-to-r from-slate-900 to-indigo-950 p-6 rounded-2xl border border-indigo-500/30 shadow-xl">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                <div>
                    <h3 class="text-base font-bold text-indigo-400 uppercase tracking-wider flex items-center gap-2">
                        <span>🚨</span> Quadro de Planejamento de Reforço (Apoio à Decisão)
                    </h3>
                    <p class="text-xs text-slate-400 mt-1">Simule abaixo a distribuição do novo efetivo para abater diretamente no déficit de cada Cia.</p>
                </div>
                <div class="flex gap-4 bg-slate-950/60 px-4 py-3 rounded-xl border border-slate-800">
                    <div class="text-center">
                        <p class="text-[10px] uppercase font-bold text-slate-400">Recém-Formados</p>
                        <p class="text-xl font-black text-emerald-400">23</p>
                    </div>
                    <div class="border-l border-slate-800"></div>
                    <div class="text-center">
                        <p class="text-[10px] uppercase font-bold text-slate-400">Apresentações</p>
                        <p class="text-xl font-black text-sky-400">7</p>
                    </div>
                    <div class="border-l border-indigo-500/30"></div>
                    <div class="text-center px-1">
                        <p class="text-[10px] uppercase font-bold text-indigo-400">Total Disponível</p>
                        <p id="reforço-disponivel" class="text-xl font-black text-indigo-400">30</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="mb-6 bg-slate-900/60 p-4 rounded-2xl border border-slate-800/80 shadow-lg">
            <p class="text-xs font-bold text-slate-400 uppercase tracking-widest mb-3 flex items-center gap-2">
                <span>🔍</span> Filtrar por Unidade / Companhia (Segmentação)
            </p>
            <div id="slicer-container" class="flex flex-wrap gap-2">
                </div>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
            <div class="bg-slate-900 p-6 rounded-2xl border border-slate-800/80 shadow-md">
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Efetivo Fixado</p>
                <p id="kpi-fixado" class="text-4xl font-black text-slate-200 mt-2">0</p>
            </div>
            <div class="bg-slate-900 p-6 rounded-2xl border border-slate-800/80 shadow-md border-l-4 border-l-sky-500">
                <p class="text-xs font-bold text-sky-400 uppercase tracking-wider">Efetivo Existente</p>
                <p id="kpi-existente" class="text-4xl font-black text-sky-400 mt-2">0</p>
            </div>
            <div class="bg-slate-900 p-6 rounded-2xl border border-slate-800/80 shadow-md border-l-4 border-l-emerald-500">
                <p class="text-xs font-bold text-emerald-400 uppercase tracking-wider">Efetivo Real</p>
                <p id="kpi-real" class="text-4xl font-black text-emerald-400 mt-2">0</p>
            </div>
            <div class="bg-slate-900 p-6 rounded-2xl border border-slate-800/80 shadow-md border-l-4 border-l-rose-500">
                <p class="text-xs font-bold text-rose-400 uppercase tracking-wider">Déficit (Claro Atual)</p>
                <div class="flex items-baseline gap-4 mt-2">
                    <p id="kpi-claro" class="text-4xl font-black text-rose-500">0</p>
                    <span class="text-slate-500">|</span>
                    <p id="kpi-restricao" class="text-2xl font-bold text-amber-500" title="Restrições Médicas">0 R.</p>
                </div>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
            <div class="bg-slate-900 p-6 rounded-2xl border border-slate-800/80 shadow-md">
                <h3 class="text-sm font-bold text-slate-300 mb-4 uppercase tracking-wider">Comparativo de Efetivos</h3>
                <div class="h-72 relative">
                    <canvas id="chartEfetivo"></canvas>
                </div>
            </div>
            <div class="bg-slate-900 p-6 rounded-2xl border border-slate-800/80 shadow-md">
                <h3 class="text-sm font-bold text-slate-300 mb-4 uppercase tracking-wider">Déficit Operacional Atual vs. Projetado</h3>
                <div class="h-72 relative">
                    <canvas id="chartDeficit"></canvas>
                </div>
            </div>
        </div>

        <div class="bg-slate-900 rounded-2xl border border-slate-800/80 overflow-hidden shadow-xl mb-12">
            <div class="px-6 py-4 border-b border-slate-800 bg-slate-900/50 flex justify-between items-center">
                <h3 class="text-sm font-bold text-slate-300 uppercase tracking-wider">Planilha Base de Distribuição e Alocação</h3>
                <span id="filter-indicator" class="text-xs text-indigo-400 font-semibold bg-indigo-950/50 px-3 py-1 rounded-md border border-indigo-900">Exibindo: Todos</span>
            </div>
            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-950 text-slate-400 text-[10px] uppercase tracking-widest font-bold border-b border-slate-800">
                            <th class="py-4 px-5">Unidade</th>
                            <th class="py-4 px-5 text-center text-rose-400">Perdeu</th>
                            <th class="py-4 px-5 text-center text-emerald-400">Recebeu</th>
                            <th class="py-4 px-5 text-center">Ef. Fixado</th>
                            <th class="py-4 px-5 text-center text-sky-400">Ef. Existente</th>
                            <th class="py-4 px-5 text-center text-amber-500">Restrição</th>
                            <th class="py-4 px-5 text-center text-emerald-400">Ef. Real</th>
                            <th class="py-4 px-5 text-center text-rose-500">Claro Atual</th>
                            <th class="py-4 px-5 text-center text-indigo-400 bg-indigo-950/30">Receber Reforço</th>
                            <th class="py-4 px-5 text-center text-emerald-400">Claro Projetado</th>
                        </tr>
                    </thead>
                    <tbody id="table-body" class="divide-y divide-slate-800/60 text-sm text-slate-300">
                        </tbody>
                </table>
            </div>
        </div>

    </div>

    <script>
        let dadosAtuais = [];
        let unidadeSelecionada = "TODOS";
        
        // Objeto para guardar a simulação de reforços alocados por Cia
        let distribuicaoReforco = {
            "1ªCia": 0, "2ªCia": 0, "3ªCia": 0, "4ªCia": 0, "5ªCia": 0, "6ªCia": 0, "Cia Ft": 0, "EM": 0
        };
        
        let chartEfetivoInstance = null;
        let chartDeficitInstance = null;

        const dadosIniciais = [
            { "UNIDADE": "1ªCia", "PERDEU": 5, "RECEBEU": 1, "EF_FIXADO": 129, "EF_EXISTENTE": 85, "RESTRICAO": 13, "EF_REAL": 72, "CLARO_REAL": 57, "CLARO_PCT": "44%" },
            { "UNIDADE": "2ªCia", "PERDEU": 1, "RECEBEU": 0, "EF_FIXADO": 91, "EF_EXISTENTE": 75, "RESTRICAO": 8, "EF_REAL": 67, "CLARO_REAL": 24, "CLARO_PCT": "26%" },
            { "UNIDADE": "3ªCia", "PERDEU": 1, "RECEBEU": 2, "EF_FIXADO": 122, "EF_EXISTENTE": 90, "RESTRICAO": 19, "EF_REAL": 71, "CLARO_REAL": 51, "CLARO_PCT": "42%" },
            { "UNIDADE": "4ªCia", "PERDEU": 4, "RECEBEU": 2, "EF_FIXADO": 146, "EF_EXISTENTE": 114, "RESTRICAO": 5, "EF_REAL": 109, "CLARO_REAL": 37, "CLARO_PCT": "25%" },
            { "UNIDADE": "5ªCia", "PERDEU": 3, "RECEBEU": 0, "EF_FIXADO": 155, "EF_EXISTENTE": 106, "RESTRICAO": 9, "EF_REAL": 97, "CLARO_REAL": 58, "CLARO_PCT": "37%" },
            { "UNIDADE": "6ªCia", "PERDEU": 0, "RECEBEU": 0, "EF_FIXADO": 146, "EF_EXISTENTE": 87, "RESTRICAO": 13, "EF_REAL": 74, "CLARO_REAL": 72, "CLARO_PCT": "49%" },
            { "UNIDADE": "Cia Ft", "PERDEU": 0, "RECEBEU": 0, "EF_FIXADO": 123, "EF_EXISTENTE": 73, "RESTRICAO": 2, "EF_REAL": 71, "CLARO_REAL": 52, "CLARO_PCT": "42%" },
            { "UNIDADE": "EM", "PERDEU": 0, "RECEBEU": 0, "EF_FIXADO": 78, "EF_EXISTENTE": 142, "RESTRICAO": 15, "EF_REAL": 127, "CLARO_REAL": -49, "CLARO_PCT": "-63%" }
        ];

        inicializarDashboard(dadosIniciais);

        function inicializarDashboard(dados) {
            dadosAtuais = dados;
            construirSlicers();
            processarEFiltrarVisuais();
        }

        function construirSlicers() {
            const container = document.getElementById('slicer-container');
            container.innerHTML = "";

            const btnTodos = criarBotaoSlicer("TODOS");
            container.appendChild(btnTodos);

            dadosAtuais.forEach(d => {
                const btn = criarBotaoSlicer(d.UNIDADE);
                container.appendChild(btn);
            });
        }

        function criarBotaoSlicer(label) {
            const btn = document.createElement('button');
            btn.innerText = label;
            if (unidadeSelecionada === label) {
                btn.className = "px-4 py-2 text-xs font-bold rounded-lg bg-indigo-600 text-white border border-indigo-500 shadow-md transition-all";
            } else {
                btn.className = "px-4 py-2 text-xs font-bold rounded-lg bg-slate-800 text-slate-400 border border-slate-700/60 hover:bg-slate-700/60 hover:text-slate-200 transition-all";
            }
            btn.onclick = () => {
                unidadeSelecionada = label;
                construirSlicers();
                processarEFiltrarVisuais();
            };
            return btn;
        }

        // Função chamada sempre que o comando altera a alocação de soldados no simulador
        window.atualizarSimulacao = function(unidade, valor) {
            distribuicaoReforco[unidade] = Number(valor) || 0;
            
            // Calcula o total alocado e atualiza o contador do cabeçalho
            let totalAlocado = Object.values(distribuicaoReforco).reduce((a, b) => a + b, 0);
            let restante = 30 - totalAlocado;
            
            const txtRestante = document.getElementById('reforço-disponivel');
            txtRestante.innerText = restante;
            
            if (restante < 0) {
                txtRestante.className = "text-xl font-black text-rose-500 animate-pulse";
            } else {
                txtRestante.className = "text-xl font-black text-indigo-400";
            }

            processarEFiltrarVisuais();
        }

        function processarEFiltrarVisuais() {
            const dadosFiltrados = unidadeSelecionada === "TODOS" 
                ? dadosAtuais 
                : dadosAtuais.filter(d => d.UNIDADE === unidadeSelecionada);

            document.getElementById('filter-indicator').innerText = `Exibindo: ${unidadeSelecionada}`;

            let tFixado = 0, tExistente = 0, tReal = 0, tClaro = 0, tRestricao = 0;
            dadosFiltrados.forEach(d => {
                tFixado += Number(d.EF_FIXADO) || 0;
                tExistente += Number(d.EF_EXISTENTE) || 0;
                tReal += Number(d.EF_REAL) || 0;
                tClaro += Number(d.CLARO_REAL) || 0;
                tRestricao += Number(d.RESTRICAO) || 0;
            });

            document.getElementById('kpi-fixado').innerText = tFixado;
            document.getElementById('kpi-existente').innerText = tExistente;
            document.getElementById('kpi-real').innerText = tReal;
            document.getElementById('kpi-claro').innerText = tClaro;
            document.getElementById('kpi-restricao').innerText = `${tRestricao} R.`;

            // Construção da Tabela
            const tbody = document.getElementById('table-body');
            tbody.innerHTML = "";

            dadosFiltrados.forEach(d => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/40 transition-colors border-b border-slate-800/40";
                
                const cReal = d.CLARO_REAL ?? 0;
                const corClaro = cReal > 0 ? 'text-rose-500 font-bold' : 'text-emerald-400 font-bold';
                
                // Cálculo projetado com base na digitação do Comando
                const alocado = distribuicaoReforco[d.UNIDADE] || 0;
                const claroProjetado = cReal - alocado;
                const corProj = claroProjetado > 0 ? 'text-rose-400' : 'text-emerald-400 font-black';

                tr.innerHTML = `
                    <td class="py-3.5 px-5 font-bold text-white">${d.UNIDADE}</td>
                    <td class="py-3.5 px-5 text-center text-rose-400 font-medium">${d.PERDEU ?? 0}</td>
                    <td class="py-3.5 px-5 text-center text-emerald-400 font-medium">${d.RECEBEU ?? 0}</td>
                    <td class="py-3.5 px-5 text-center font-medium text-slate-400">${d.EF_FIXADO ?? 0}</td>
                    <td class="py-3.5 px-5 text-center font-bold text-sky-400">${d.EF_EXISTENTE ?? 0}</td>
                    <td class="py-3.5 px-5 text-center text-amber-500 font-medium">${d.RESTRICAO ?? 0}</td>
                    <td class="py-3.5 px-5 text-center font-bold text-emerald-400">${d.EF_REAL ?? 0}</td>
                    <td class="py-3.5 px-5 text-center ${corClaro}">${cReal}</td>
                    <td class="py-2 px-5 text-center bg-indigo-950/20">
                        <input type="number" min="0" value="${alocado}" 
                            oninput="atualizarSimulacao('${d.UNIDADE}', this.value)"
                            class="w-16 bg-slate-950 text-center text-indigo-400 font-bold border border-indigo-500/40 rounded py-1 focus:outline-none focus:border-indigo-400" />
                    </td>
                    <td class="py-3.5 px-5 text-center bg-slate-950/30 font-bold ${corProj}">${claroProjetado}</td>
                `;
                tbody.appendChild(tr);
            });

            // Linha de Totais Consolidados
            if (unidadeSelecionada === "TODOS") {
                const trTotal = document.createElement('tr');
                trTotal.className = "bg-slate-950/80 font-bold text-xs border-t border-slate-700 text-slate-200";
                const totalPerdeu = dadosAtuais.reduce((acc, d) => acc + (Number(d.PERDEU)||0), 0);
                const totalRecebeu = dadosAtuais.reduce((acc, d) => acc + (Number(d.RECEBEU)||0), 0);
                const totalAlocado = Object.values(distribuicaoReforco).reduce((a, b) => a + b, 0);

                trTotal.innerHTML = `
                    <td class="py-4 px-5">TOTAL CONSOLIDADO</td>
                    <td class="py-4 px-5 text-center text-rose-400">${totalPerdeu}</td>
                    <td class="py-4 px-5 text-center text-emerald-400">${totalRecebeu}</td>
                    <td class="py-4 px-5 text-center">${tFixado}</td>
                    <td class="py-4 px-5 text-center text-sky-400">${tExistente}</td>
                    <td class="py-4 px-5 text-center text-amber-400">${tRestricao}</td>
                    <td class="py-4 px-5 text-center text-emerald-400">${tReal}</td>
                    <td class="py-4 px-5 text-center text-rose-500">${tClaro}</td>
                    <td class="py-4 px-5 text-center bg-indigo-950 text-indigo-400 font-black">${totalAlocado} aloc.</td>
                    <td class="py-4 px-5 text-center bg-slate-950 font-black text-white">${tClaro - totalAlocado}</td>
                `;
                tbody.appendChild(trTotal);
            }

            // Gráficos
            const labels = dadosFiltrados.map(d => d.UNIDADE);
            const arrFixado = dadosFiltrados.map(d => d.EF_FIXADO ?? 0);
            const arrExistente = dadosFiltrados.map(d => d.EF_EXISTENTE ?? 0);
            const arrReal = dadosFiltrados.map(d => d.EF_REAL ?? 0);
            
            const arrClaroOriginal = dadosFiltrados.map(d => d.CLARO_REAL ?? 0);
            const arrClaroProjetado = dadosFiltrados.map(d => (d.CLARO_REAL ?? 0) - (distribuicaoReforco[d.UNIDADE] || 0));

            Chart.defaults.color = '#94a3b8';
            Chart.defaults.borderColor = '#334155';

            if (chartEfetivoInstance) chartEfetivoInstance.destroy();
            if (chartDeficitInstance) chartDeficitInstance.destroy();

            const ctx1 = document.getElementById('chartEfetivo').getContext('2d');
            chartEfetivoInstance = new Chart(ctx1, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [
                        { label: 'Fixado', data: arrFixado, backgroundColor: '#475569' },
                        { label: 'Existente', data: arrExistente, backgroundColor: '#0ea5e9' },
                        { label: 'Real', data: arrReal, backgroundColor: '#10b981' }
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });

            // Gráfico 2 modificado: Agora compara a falta de pessoal Atual contra a Projetada
            const ctx2 = document.getElementById('chartDeficit').getContext('2d');
            chartDeficitInstance = new Chart(ctx2, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [
                        { type: 'bar', label: 'Déficit Atual (Claro)', data: arrClaroOriginal, backgroundColor: '#f43f5e' },
                        { type: 'bar', label: 'Déficit Após Alocação', data: arrClaroProjetado, backgroundColor: '#3b82f6' }
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        // Importação de CSV
        document.getElementById('csv_file').addEventListener('change', function(e) {
            const arquivo = e.target.files[0];
            if (!arquivo) return;

            const leitor = new FileReader();
            leitor.onload = function(evt) {
                const dadosLidos = processarCSV(evt.target.result);
                if (dadosLidos && dadosLidos.length > 0) {
                    unidadeSelecionada = "TODOS";
                    inicializarDashboard(dadosLidos);
                } else {
                    alert("Estrutura incorreta. Use ponto e vírgula (;)");
                }
            };
            leitor.readAsText(arquivo, 'UTF-8');
        });

        function processarCSV(texto) {
            const linhas = texto.split(/\r?\n/).map(l => l.trim()).filter(l => l.length > 0);
            if (linhas.length < 2) return [];

            const cabecalhos = lines[0].split(';').map(c => c.trim().toUpperCase().replace(/"/g, ''));
            const listaObjetos = [];

            for (let i = 1; i < lines.length; i++) {
                const colunas = lines[i].split(';');
                if (!colunas[0] || colunas[0].toUpperCase() === 'TOTAL') continue;

                let obj = {};
                colunas.forEach((valor, index) => {
                    if (index >= cabecalhos.length) return;
                    let header = cabecalhos[index];
                    let valLimpo = valor.trim().replace(/"/g, '');

                    if (header.includes("UNIDADE")) header = "UNIDADE";
                    if (header.includes("FIXADO")) header = "EF_FIXADO";
                    if (header.includes("EXISTENTE")) header = "EF_EXISTENTE";
                    if (header.includes("RESTRI")) header = "RESTRICAO";
                    if (header.includes("EF. REAL") || header === "EF_REAL") header = "EF_REAL";
                    if (header.includes("CLARO REAL") || header === "CLARO_REAL") header = "CLARO_REAL";
                    if (header === "CLARO2" || header === "CLARO_PCT") header = "CLARO_PCT";

                    if (valLimpo.includes('%')) {
                        obj[header] = valLimpo;
                    } else {
                        let num = Number(valLimpo.replace(',', '.'));
                        obj[header] = !isNaN(num) && valLimpo !== '' ? num : valLimpo;
                    }
                });

                if (obj.UNIDADE) listaObjetos.push(obj);
            }
            return listaObjetos;
        }
    </script>
</body>
</html>
