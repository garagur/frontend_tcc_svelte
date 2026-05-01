<script>
  import MainScreen from './MainScreen.svelte';
  import CadastroServidor from './CadastroServidor.svelte'; // <-- Importamos a tela que criamos antes

  let telaAtual = 'login'; 
  let loginMatricula = '';
  let loginSenha = '';

  function entrar() {
    if (loginMatricula && loginSenha) {
      telaAtual = 'home';
    } else {
      alert("Preencha a matrícula e a senha!");
    }
  }

  function deslogar() {
    telaAtual = 'login';
    loginMatricula = '';
    loginSenha = '';
  }

  // Função genérica que será passada para as telas trocarem o estado principal
  function mudarTela(novaTela) {
    telaAtual = novaTela;
  }
</script>

{#if telaAtual === 'login'}
  <div class="login-wrapper">
    <div class="card">
      <h2>Acesso ao Sistema</h2>
      <form on:submit|preventDefault={entrar}>
        <div class="field">
          <label for="m">Matrícula</label>
          <input id="m" type="text" bind:value={loginMatricula} placeholder="000000" required />
        </div>
        <div class="field">
          <label for="s">Senha</label>
          <input id="s" type="password" bind:value={loginSenha} placeholder="******" required />
        </div>
        <button type="submit" class="btn-main">Entrar</button>
      </form>
    </div>
  </div>

{:else if telaAtual === 'home'}
  <!-- Passamos a função mudarTela para a prop onNavigate -->
  <MainScreen 
    matricula={loginMatricula} 
    aoSair={deslogar} 
    onNavigate={mudarTela} 
  />

{:else if telaAtual === 'cadastroServidor'}
  <!-- Quando clicar em "Sair" lá no cadastro, ele volta para a 'home' em vez do login -->
  <CadastroServidor onSair={() => mudarTela('home')} />
{/if}

<style>
  :global(body) {
    margin: 0;
    background: linear-gradient(180deg, #0ea5e9 0%, #172554 100%);
    background-attachment: fixed;
    width: 100%;
    min-height: 100vh;
    display: block;
    font-family: 'Inter', sans-serif;
  }
  :global(#app) {
    width: 100%;
    margin: 0;
    padding: 0;
  }
  .login-wrapper {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
  }

  .card {
    /* Card agora é branco */
    background: #ffffff; 
    padding: 2.5rem;
    border-radius: 32px;
    width: 100%;
    max-width: 400px;
    /* Sombra mais suave para combinar com o fundo claro */
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); 
  }

  h2 { 
    color: #0284c7; 
    text-align: center; 
    margin-bottom: 1.5rem;
  }

  form { display: flex; flex-direction: column; gap: 1.2rem; }

  .field { display: flex; flex-direction: column; gap: 0.5rem; }

  label {
    font-weight: 600;
    font-size: 0.9rem;
    color: #64748b;
  }

  input {
    padding: 0.75rem;
    background: #f8fafc; 
    border: 1px solid #cbd5e1;
    color: #1e293b;
    border-radius: 6px;
  }

  input:focus {
    outline: none;
    border-color: #38bdf8;
    background: #fff;
  }

  .btn-main {
    background: #0284c7;
    color: white;
    border: none;
    padding: 0.8rem;
    border-radius: 6px;
    cursor: pointer;
    font-weight: bold;
    transition: background 0.2s;
  }

  .btn-main:hover {
    background: #0369a1;
  }

  .btn-link {
    background: none;
    border: none;
    color: #64748b;
    text-decoration: underline;
    cursor: pointer;
    font-size: 0.85rem;
  }
</style>