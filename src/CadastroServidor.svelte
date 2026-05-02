<script>
  export let onSair;

  let novoServidor = {
    nome: '',
    email: '',
    cargo: '',
    matricula: ''
  };

  // Lista simulada que virá do Laravel no futuro
  let servidores = [
    { nome: 'Carlos Oliveira', email: 'carlos@escola.com', cargo: 'Técnico', matricula: '12345' },
    { nome: 'Ana Souza', email: 'ana@escola.com', cargo: 'Instrutora', matricula: '67890' }
  ];

  function salvarServidor() {
    const index = servidores.findIndex(s => s.matricula === novoServidor.matricula);
    
    if (index >= 0) {
      servidores[index] = { ...novoServidor }; 
      alert(`Servidor ${novoServidor.nome} atualizado!`);
    } else {
      servidores = [...servidores, { ...novoServidor }];
      alert(`Servidor ${novoServidor.nome} cadastrado!`);
    }
    
    novoServidor = { nome: '', email: '', cargo: '', matricula: '' };
  }

  function editarServidor(servidor) {
    novoServidor = { ...servidor };
  }

  function excluirServidor(matricula) {
    if(confirm("Tem certeza que deseja excluir este servidor?")) {
      servidores = servidores.filter(s => s.matricula !== matricula);
    }
  }
</script>

<div class="scaffold">
  <header class="app-bar">
    <div class="title-section">
      <h1>Portal de Agendamento</h1>
      <span>Cadastro de Servidor / Usuário</span>
    </div>
    <div class="actions-section">
      <button class="btn-icon" on:click={onSair} title="Voltar para a Home">
        <span class="material-symbols-outlined">arrow_back</span>
      </button>
    </div>
  </header>

  <!-- Container Principal: Lado a Lado -->
  <main class="body-content">
    
    <!-- ESQUERDA: Formulário -->
    <div class="card form-card">
      <div class="card-header">
        <span class="material-symbols-outlined icon-large">
          {novoServidor.matricula ? 'manage_accounts' : 'person_add'}
        </span>
      </div>

      <form on:submit|preventDefault={salvarServidor}>
        <!-- Como o form agora fica na lateral, 1 coluna fica melhor -->
        <div class="field">
          <label>Nome Completo</label>
          <input type="text" bind:value={novoServidor.nome} placeholder="Ex: João da Silva" required />
        </div>
        <div class="field">
          <label>E-mail</label>
          <input type="email" bind:value={novoServidor.email} placeholder="joao@escola.com" required />
        </div>
        <div class="field">
          <label>Cargo</label>
          <select bind:value={novoServidor.cargo} required>
            <option value="" disabled selected>Selecione um cargo...</option>
            <option value="admin">Admin</option>
            <option value="servidor">Servidor</option>
            <option value="educador">Educador</option>
          </select>
        </div>
        <div class="field">
          <label>Matrícula</label>
          <input type="text" bind:value={novoServidor.matricula} placeholder="000000" required />
        </div>

        <div class="bottom-action">
          <button type="submit" class="btn-primary">
            <span class="material-symbols-outlined">save</span>
            {novoServidor.matricula ? 'Atualizar' : 'Salvar'}
          </button>
        </div>
      </form>
    </div>

    <!-- DIREITA: Planilha -->
    <div class="card table-card">
      <div class="table-header-title">
        <div class="title-left">
          <span class="material-symbols-outlined text-blue">group</span>
          <h3>Servidores Cadastrados</h3>
        </div>
        <div class="badge">{servidores.length} registros</div>
      </div>

      <div class="table-wrapper">
        <div class="table-header">
          <div class="th flex-2">Nome</div>
          <div class="th flex-2">E-mail</div>
          <div class="th flex-1">Cargo</div> <!-- Nova coluna adicionada -->
          <div class="th flex-1">Matrícula</div>
          <div class="th flex-1">Ações</div>
        </div>
        
        <div class="table-body">
          {#each servidores as s, index}
            <div class="table-row {index % 2 === 0 ? 'even' : 'odd'}">
              <div class="td flex-2">
                <span class="material-symbols-outlined icon-tiny">person</span>
                <span class="text-truncate">{s.nome}</span>
              </div>
              
              <div class="td flex-2">
                <span class="material-symbols-outlined icon-tiny">mail</span>
                <span class="text-truncate">{s.email}</span>
              </div>

              <!-- Nova célula com o Cargo -->
              <div class="td flex-1">
                <span class="badge-cargo">{s.cargo}</span>
              </div>
              
              <div class="td flex-1">
                <span class="badge-matricula">{s.matricula}</span>
              </div>
              
              <div class="td flex-1 action-cell">
                <button class="btn-action edit" on:click={() => editarServidor(s)} title="Editar">
                  <span class="material-symbols-outlined">edit</span>
                </button>
                <button class="btn-action delete" on:click={() => excluirServidor(s.matricula)} title="Excluir">
                  <span class="material-symbols-outlined">delete</span>
                </button>
              </div>
            </div>
          {/each}
        </div>
      </div>
    </div>

  </main>
</div>

<style>
  :root {
    --blue-primary: #2196F3;
    --blue-dark: #1976D2;
    --blue-light: #BBDEFB;
    --blue-50: #E3F2FD;
    --white: #FFFFFF;
    --text-dark: #424242;
    --bg-input: #f8fafc;
    --border-input: #cbd5e1;
    --danger: #ef4444;
    --warning: #f59e0b;
  }

  * { box-sizing: border-box; }
  .scaffold { display: flex; flex-direction: column; height: 100vh; font-family: 'Inter', Arial, sans-serif; overflow: hidden; }

  /* APP BAR */
  .app-bar { 
    background-color: var(--white);
    padding: 12px 24px; 
    display: flex; 
    flex-direction: row; 
    justify-content: space-between; 
    align-items: center; 
    box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    z-index: 10;
  }
 .title-section { display: flex; flex-direction: column; }
  .title-section h1 { 
    margin: 0; 
    font-size: 18px; 
    color: var(--blue-primary);
  }
  .title-section span { 
    font-size: 11px; 
    color: #64748b;
    margin-top: 2px; 
  }
  .btn-icon { 
    background: none; 
    border: none; 
    color: var(--blue-primary);
    cursor: pointer; 
    padding: 8px; 
    border-radius: 50%; 
    transition: background 0.2s; 
    display: flex; 
    align-items: center; 
    justify-content: center; 
  }
  .btn-icon:hover { 
    background: #cdcdcd;
  }

 
  .body-content {
    flex: 1;
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    padding: 24px;
    gap: 24px;
    width: 100%;
    max-width: 1400px;
    margin: 0 auto;
    overflow: hidden;
  }

  .card {
    background: #ffffff;
    border-radius: 24px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.05);
  }
  .form-card { 
    width: 350px;
    flex-shrink: 0;
    padding: 2rem; 
    overflow-y: auto;
    max-height: 100%;
  }
  
  .table-card { 
    flex: 1;
    display: flex; 
    flex-direction: column; 
    height: 100%;
    padding-bottom: 24px;
  }

  /* HEADER DO FORMULÁRIO */
  .card-header { display: flex; flex-direction: column; align-items: center; margin-bottom: 1rem; }
  .icon-large { font-size: 32px; color: var(--blue-primary); margin-bottom: 4px; }
  .card-header h2 { margin: 0; color: var(--blue-primary); font-size: 18px; }

  /* FORMULÁRIO E INPUTS */
  form { display: flex; flex-direction: column; gap: 1.2rem; }
  .field { display: flex; flex-direction: column; gap: 0.5rem; }
  label { font-weight: 600; font-size: 0.85rem; color: #64748b; }
 input { 
    padding: 0.65rem; 
    background: var(--bg-input); 
    border: 1px solid var(--border-input); 
    color: #1e293b; 
    border-radius: 8px; 
    font-size: 0.9rem; 
    transition: border-color 0.2s; 
    width: 100%; 
  }
  input:focus { 
    border-color: var(--blue-primary); 
    outline: none; 
    background: #fff; 
  }
  select {
    padding: 0.65rem;
    background-color: var(--bg-input);
    border: 1px solid var(--border-input);
    color: #1e293b;
    border-radius: 8px;
    font-size: 0.9rem;
    width: 100%;
    font-family: inherit;
    cursor: pointer;
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%2364748b' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
    background-repeat: no-repeat;
    background-position: right 0.75rem center;
    background-size: 1rem;
  }
  select:focus {
    border-color: var(--blue-primary);
    outline: none;
    background-color: #fff;
  }
  .bottom-action { margin-top: 1rem; display: flex; justify-content: center; }
  .btn-primary { width: 100%; background-color: var(--blue-primary); color: var(--white); border: none; border-radius: 12px; padding: 14px; font-size: 15px; font-weight: bold; display: flex; justify-content: center; align-items: center; gap: 8px; cursor: pointer; transition: background 0.2s; }
  .btn-primary:hover { background-color: var(--blue-dark); }

  /* CABEÇALHO DA TABELA */
  .table-header-title { padding: 24px 24px 16px; display: flex; justify-content: space-between; align-items: center; }
  .title-left { display: flex; align-items: center; gap: 8px; }
  .text-blue { color: var(--blue-primary); }
  .table-header-title h3 { margin: 0; font-size: 18px; color: var(--blue-primary); }
  .badge { background-color: var(--blue-50); border: 1px solid var(--blue-light); color: var(--blue-dark); padding: 4px 10px; border-radius: 12px; font-size: 11px; font-weight: bold;}
  
  /* ESTRUTURA DA PLANILHA (Rolagem interna) */
  .table-wrapper { 
    margin: 0 24px; 
    display: flex; 
    flex-direction: column; 
    border: 1px solid var(--blue-light); 
    border-radius: 12px;
    flex: 1; /* Ocupa o resto do card verticalmente */
    overflow: hidden; /* Essencial para a rolagem funcionar corretamente */
  }
  .table-header { background-color: var(--blue-primary); color: var(--white); display: flex; flex-shrink: 0; }
  .table-header .th { padding: 12px 8px; font-size: 12px; font-weight: bold; text-align: center; }
  
  .table-body { 
    flex: 1; 
    overflow-y: auto; /* Rolagem apenas nas linhas da tabela! */
    background: white;
  }
  
  .table-row { display: flex; transition: background 0.2s; border-bottom: 1px solid #f1f5f9; }
  .table-row:last-child { border-bottom: none; }
  .table-row:hover { background-color: #f1f8ff; }
  .table-row.even { background-color: var(--white); }
  .table-row.odd { background-color: var(--blue-50); }
  
  .td { padding: 12px 6px; display: flex; flex-direction: column; align-items: center; justify-content: center; font-size: 12px; color: var(--text-dark); }
  .flex-2 { flex: 2; width: 0; }
  .flex-1 { flex: 1; width: 0; }
  .text-truncate { display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; margin-top: 2px; text-align: center;}
  .icon-tiny { font-size: 14px; color: #42A5F5; }
  .badge-matricula { background: #e2e8f0; padding: 4px 8px; border-radius: 6px; font-weight: bold; color: #475569; }

  /* BOTÕES DE AÇÃO NA TABELA */
  .action-cell { display: flex; flex-direction: row !important; gap: 12px; align-items: center; justify-content: center; }
  .btn-action { background: none; border: none; cursor: pointer; padding: 6px; border-radius: 8px; transition: all 0.2s; display: flex; align-items: center; justify-content: center;}
  .btn-action.edit { color: var(--warning); background-color: #fef3c7;}
  .btn-action.edit:hover { background-color: #fde68a; }
  .btn-action.delete { color: var(--danger); background-color: #fee2e2;}
  .btn-action.delete:hover { background-color: #fecaca; }
  
  /* Quebra para celular/telas muito pequenas */
  @media (max-width: 900px) {
    .body-content { flex-direction: column; }
    .form-card { width: 100%; max-width: none; }
    .table-card { overflow: visible; height: auto; }
    .table-body { overflow-y: visible; }
  }
</style>