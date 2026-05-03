<script>
  import MainScreen from './MainScreen.svelte';
  import CadastroServidor from './CadastroServidor.svelte';
  import PrimeiroLogin from './PrimeiroLogin.svelte';

  let telaAtual = 'login';
  let loginMatricula = '';
  let loginSenha = '';
  let erroLogin = '';
  let carregando = false;

  // Token e dados do usuário ficam salvos aqui
  let token = '';
  let usuarioLogado = null;

  async function entrar() {
    if (!loginMatricula || !loginSenha) {
      erroLogin = 'Preencha a matrícula e a senha!';
      return;
    }

    carregando = true;
    erroLogin = '';

    try {
      const resposta = await fetch('http://localhost:8000/api/eafinware/auth/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
        },
        body: JSON.stringify({
          matricula: loginMatricula,
          password: loginSenha,
        }),
      });

      const dados = await resposta.json();

      if (!resposta.ok) {
        // A API retornou erro (401, 422, etc.)
        erroLogin = dados.message || 'Credenciais inválidas.';
        return;
      }

      // Salva token e usuário
      token = dados.token;
      usuarioLogado = dados.user;

      // Guarda no localStorage para persistir ao recarregar a página
      localStorage.setItem('token', token);
      localStorage.setItem('usuario', JSON.stringify(usuarioLogado));

      // Lógica de navegação igual à anterior
      if (usuarioLogado.cargo === 'primeiroLogin') {
        mudarTela('primeiroLogin');
      } else {
        mudarTela('home');
      }

    } catch (e) {
      erroLogin = 'Não foi possível conectar ao servidor. Verifique sua conexão.';
    } finally {
      carregando = false;
    }
  }

  function deslogar() {
    telaAtual = 'login';
    loginMatricula = '';
    loginSenha = '';
    token = '';
    usuarioLogado = null;
    localStorage.removeItem('token');
    localStorage.removeItem('usuario');
  }

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

        <!-- Mensagem de erro da API -->
        {#if erroLogin}
          <p class="erro">{erroLogin}</p>
        {/if}

        <button type="submit" class="btn-main" disabled={carregando}>
          {carregando ? 'Entrando...' : 'Entrar'}
        </button>
      </form>
    </div>
  </div>

{:else if telaAtual === 'primeiroLogin'}
  <PrimeiroLogin 
    matricula={loginMatricula} 
    onConcluido={deslogar} 
  />

{:else if telaAtual === 'home'}
  <MainScreen 
    matricula={loginMatricula}
    usuario={usuarioLogado}
    token={token}
    aoSair={deslogar} 
    onNavigate={mudarTela} 
  />

{:else if telaAtual === 'cadastroServidor'}
  <CadastroServidor 
    onSair={() => mudarTela('home')} 
    {token}/>
{/if}

<style>
  :global(body) {
    margin: 0;
    background: linear-gradient(135deg, #0ea5e9 0%, #172554 100%);
    background-attachment: fixed;
    min-height: 100vh;
  }
  :global(html, body) {
    margin: 0 !important;
    padding: 0 !important;
    width: 100% !important;
    min-height: 100vh !important;
  }
:global(#app) {
    width: 100% !important; /* Essa era a peça que faltava */
    max-width: 100% !important;
    margin: 0 !important;
    padding: 0 !important;
    display: block !important; 
  }
  .login-wrapper { height: 100vh; display: flex; justify-content: center; align-items: center; width: 100%; font-family: 'Inter', sans-serif;}
  .card { background: #ffffff; padding: 2.5rem; border-radius: 24px; width: 100%; max-width: 400px; box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); }
  h2 { color: #0284c7; text-align: center; margin-bottom: 1.5rem;}
  form { display: flex; flex-direction: column; gap: 1.2rem; }
  .field { display: flex; flex-direction: column; gap: 0.5rem; }
  label { font-weight: 600; font-size: 0.9rem; color: #64748b; }
  input { padding: 0.75rem; background: #f8fafc; border: 1px solid #cbd5e1; color: #1e293b; border-radius: 8px; }
  input:focus { outline: none; border-color: #38bdf8; background: #fff; }
  .btn-main { background: #0284c7; color: white; border: none; padding: 0.8rem; border-radius: 12px; cursor: pointer; font-weight: bold; transition: background 0.2s; }
  .btn-main:hover { background: #0369a1; }
</style>