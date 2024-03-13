<script lang="ts">
    import { page } from '$app/stores';
    import { goto } from '$app/navigation';
  
    // Acessando parâmetros da URL
    const queryParams = $page.url.searchParams;
    const nome = queryParams.get('nome');
    const telefone = queryParams.get('telefone');
    const email = queryParams.get('email');
  </script>
  
  <div class="candidate-info">
    <h1>Informações do Candidato</h1>
    <p>Nome: {nome}</p>
    <p>Telefone: {telefone}</p>
    <p>Email: {email}</p>
    <button class="btn" on:click={() => goto('/')}>Voltar ao Desafio</button>
  </div>
  
  <style>
    .candidate-info {
      /* Adicione seu CSS aqui */
    }
  </style>
  