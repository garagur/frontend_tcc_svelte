<script>
  // Recebe a matrícula para enviar para a API saber de quem é a senha
  export let matricula = ''; 
  // Função que o App.svelte vai passar para voltar à tela inicial
  export let onConcluido; 

  let novaSenha = '';
  let confirmarSenha = '';
  let erroMsg = '';

  function salvarNovaSenha() {
    // 1. Conferencia se as senhas são iguais
    if (novaSenha !== confirmarSenha) {
      erroMsg = "As senhas não coincidem. Digite novamente.";
      return;
    }

    // Opcional: Conferência de tamanho mínimo (ex: 6 caracteres)
    if (novaSenha.length < 6) {
      erroMsg = "A senha deve ter pelo menos 6 caracteres.";
      return;
    }

    // Limpa a mensagem de erro se tudo estiver certo
    erroMsg = '';

    // 2. Aqui você fará o POST/PUT para o Laravel
    console.log(`Enviando nova senha para a matrícula: ${matricula}`);
    
    // Simulação do sucesso
    alert("Senha cadastrada com sucesso! Faça login com sua nova senha.");
    
    // 3. Volta para a tela de login
    onConcluido();
  }
</script>

<div class="login-wrapper">
  <div class="card">
    <div class="header-icon">
      <span class="material-symbols-outlined icon-lock">lock_reset</span>
    </div>
    <h2>Primeiro Acesso</h2>
    <p class="subtitle">Por segurança, cadastre uma nova senha para acessar o portal escolar.</p>

    <form on:submit|preventDefault={salvarNovaSenha}>
      
      <div class="field">
        <label for="nova">Nova Senha</label>
        <input id="nova" type="password" bind:value={novaSenha} placeholder="Mínimo 6 caracteres" required />
      </div>

      <div class="field">
        <label for="confirma">Confirmar Senha</label>
        <input id="confirma" type="password" bind:value={confirmarSenha} placeholder="Repita a nova senha" required />
      </div>

      <!-- Exibe a mensagem de erro em vermelho se houver -->
      {#if erroMsg}
        <div class="error-box">
          <span class="material-symbols-outlined">error</span>
          {erroMsg}
        </div>
      {/if}

      <button type="submit" class="btn-main">Salvar e Voltar</button>
      
      <button type="button" class="btn-cancel" on:click={onConcluido}>
        Cancelar
      </button>
    </form>
  </div>
</div>

<style>
  /* Aproveitando as cores globais que você já usa */
  .login-wrapper { height: 100vh; display: flex; justify-content: center; align-items: center; width: 100%; font-family: 'Inter', sans-serif;}
  .card { background: #ffffff; padding: 2.5rem; border-radius: 24px; width: 100%; max-width: 400px; box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); display: flex; flex-direction: column; align-items: center;}
  
  .header-icon { background: #e0f2fe; padding: 16px; border-radius: 50%; margin-bottom: 12px; }
  .icon-lock { font-size: 36px; color: #0284c7; }
  
  h2 { color: #0284c7; text-align: center; margin: 0 0 8px 0; }
  .subtitle { text-align: center; font-size: 0.85rem; color: #64748b; margin-bottom: 1.5rem; line-height: 1.4;}
  
  form { width: 100%; display: flex; flex-direction: column; gap: 1rem; }
  .field { display: flex; flex-direction: column; gap: 0.4rem; }
  label { font-weight: 600; font-size: 0.85rem; color: #64748b; }
  input { padding: 0.8rem; background: #f8fafc; border: 1px solid #cbd5e1; color: #1e293b; border-radius: 8px; font-size: 0.95rem; transition: border-color 0.2s;}
  input:focus { outline: none; border-color: #38bdf8; background: #fff; }
  
  .error-box { background: #fef2f2; color: #ef4444; padding: 10px; border-radius: 8px; font-size: 0.85rem; display: flex; align-items: center; gap: 6px; font-weight: 500;}
  .error-box span { font-size: 18px; }

  .btn-main { background: #0284c7; color: white; border: none; padding: 0.9rem; border-radius: 12px; cursor: pointer; font-weight: bold; font-size: 1rem; transition: background 0.2s; margin-top: 0.5rem;}
  .btn-main:hover { background: #0369a1; }
  
  .btn-cancel { background: transparent; border: none; color: #64748b; font-weight: 600; font-size: 0.9rem; cursor: pointer; padding: 0.5rem; transition: color 0.2s;}
  .btn-cancel:hover { color: #0f172a; }
</style>