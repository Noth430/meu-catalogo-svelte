<script>
  let busca = ''
  let filtroGenero = 'Todos'
  let favoritos = []
  let mostrarFavoritos = false

  const filmes = [
    { id: 1, titulo: 'Interestelar', genero: 'Ficção Científica', ano: 2014, nota: 9.0, emoji: '🚀', sinopse: 'Um grupo de astronautas viaja através de um buraco de minhoca em busca de um novo lar para a humanidade.' },
    { id: 2, titulo: 'Parasita', genero: 'Drama', ano: 2019, nota: 8.6, emoji: '🏠', sinopse: 'Uma família pobre se infiltra na vida de uma família rica com consequências inesperadas.' },
    { id: 3, titulo: 'Matrix', genero: 'Ação', ano: 1999, nota: 8.7, emoji: '💊', sinopse: 'Um hacker descobre que a realidade é uma simulação criada por máquinas.' },
    { id: 4, titulo: 'O Poderoso Chefão', genero: 'Drama', ano: 1972, nota: 9.2, emoji: '🎩', sinopse: 'A saga de uma família mafiosa ítalo-americana e suas lutas pelo poder.' },
    { id: 5, titulo: 'Vingadores: Ultimato', genero: 'Ação', ano: 2019, nota: 8.4, emoji: '🦸', sinopse: 'Os heróis restantes tentam reverter o estalo de Thanos.' },
    { id: 6, titulo: 'Coringa', genero: 'Drama', ano: 2019, nota: 8.4, emoji: '🃏', sinopse: 'A origem do vilão mais icônico de Gotham.' },
    { id: 7, titulo: 'Blade Runner 2049', genero: 'Ficção Científica', ano: 2017, nota: 8.0, emoji: '🌆', sinopse: 'Um novo blade runner descobre um segredo que pode mergulhar a sociedade no caos.' },
    { id: 8, titulo: 'Gladiador', genero: 'Ação', ano: 2000, nota: 8.5, emoji: '⚔️', sinopse: 'Um general romano traído busca vingança como gladiador na arena.' },
    { id: 9, titulo: 'A Origem', genero: 'Ficção Científica', ano: 2010, nota: 8.8, emoji: '🌀', sinopse: 'Um ladrão que rouba segredos através de sonhos compartilhados.' },
    { id: 10, titulo: 'Panico 6', genero: 'Terro', ano: 2023, nota: 9.9, emoji: '🔪', sinopse: 'Sobreviventes do último massacre em Woodsboro, Sam, Tara, Chad e Mindy mudam-se para Nova York, mas acabam sendo caçados por um novo e impiedoso Ghostface' },
    { id: 11, titulo: 'O Protetor', genero: 'Ação', ano: 2014, nota: 8.9, emoji: '🔫', sinopse: 'Um ex-agente secreto que forjou a própria morte sai da aposentadoria para proteger uma jovem e enfrenta gangsters russos violentos..' },
    { id: 12, titulo: 'Homem Aranha 4', genero: 'Ação', ano: 2019, nota: 9.8, emoji: '🕷️', sinopse: 'Quatro anos após Sem Volta Para Casa, um Peter Parker solitário e esquecido tenta reconstruir sua vida em Nova York.' },
  ]

  const generos = ['Todos', ...new Set(filmes.map(f => f.genero))]

  function toggleFavorito(id) {
    if (favoritos.includes(id)) {
      favoritos = favoritos.filter(f => f !== id)
    } else {
      favoritos = [...favoritos, id]
    }
  }

  $: filmesFiltrados = filmes.filter(f => {
    const matchBusca = f.titulo.toLowerCase().includes(busca.toLowerCase())
    const matchGenero = filtroGenero === 'Todos' || f.genero === filtroGenero
    const matchFavoritos = !mostrarFavoritos || favoritos.includes(f.id)
    return matchBusca && matchGenero && matchFavoritos
  })
</script>

<div class="app">
  <header>
    <h1>🎬 CineList</h1>
    <p class="subtitle">Seu catálogo pessoal de filmes</p>
    <p class="contador">{filmesFiltrados.length} filme(s) • {favoritos.length} favorito(s) ❤️</p>
  </header>

  <div class="controles">
    <input
      type="text"
      placeholder="🔍 Buscar filme..."
      bind:value={busca}
      class="input-busca"
    />
    <div class="filtros">
      {#each generos as genero}
        <button
          class="btn-filtro"
          class:ativo={filtroGenero === genero}
          on:click={() => filtroGenero = genero}
        >
          {genero}
        </button>
      {/each}
      <button
        class="btn-filtro"
        class:ativo={mostrarFavoritos}
        on:click={() => mostrarFavoritos = !mostrarFavoritos}
      >
        {mostrarFavoritos ? '❤️ Favoritos' : '🤍 Todos'}
      </button>
    </div>
  </div>

  {#if filmesFiltrados.length === 0}
    <div class="vazio">
      🎭 Nenhum filme encontrado...
    </div>
  {:else}
    <div class="grid">
      {#each filmesFiltrados as filme (filme.id)}
        <div class="card" class:favorito={favoritos.includes(filme.id)}>
          <div class="card-emoji">{filme.emoji}</div>
          <div class="card-body">
            <div class="card-header">
              <h3>{filme.titulo}</h3>
              <button
                class="btn-fav"
                class:ativo={favoritos.includes(filme.id)}
                on:click={() => toggleFavorito(filme.id)}
              >
                {favoritos.includes(filme.id) ? '❤️' : '🤍'}
              </button>
            </div>
            <div class="tags">
              <span class="tag-genero">{filme.genero}</span>
              <span class="tag-ano">{filme.ano}</span>
              <span class="tag-nota">⭐ {filme.nota}</span>
            </div>
            <p class="sinopse">{filme.sinopse}</p>
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>

<style>
  .app {
    max-width: 800px;
    margin: 0 auto;
    padding: 24px 16px;
    font-family: 'Segoe UI', sans-serif;
    color: #e6e6e6;
    min-height: 100vh;
  }

  :global(body) {
    background: #0d1117;
    margin: 0;
  }

  header {
    text-align: center;
    margin-bottom: 32px;
  }

  h1 {
    font-size: 36px;
    margin-bottom: 4px;
    background: linear-gradient(135deg, #13dcf7, #000203);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .subtitle {
    color: #8b949e;
    font-size: 14px;
    margin-bottom: 6px;
  }

  .contador {
    color: #58a6ff;
    font-size: 13px;
    font-weight: 600;
  }

  .controles {
    margin-bottom: 24px;
  }

  .input-busca {
    width: 100%;
    padding: 14px 18px;
    border-radius: 12px;
    border: 2px solid #30363d;
    background: #161b22;
    color: #fff;
    font-size: 15px;
    outline: none;
    transition: border 0.2s;
    margin-bottom: 12px;
    font-family: inherit;
  }

  .input-busca:focus {
    border-color: #09d6fa;
  }

  .filtros {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }

  .btn-filtro {
    padding: 7px 16px;
    border-radius: 20px;
    border: 1px solid #30363d;
    background: transparent;
    color: #8b949e;
    font-size: 13px;
    cursor: pointer;
    transition: all 0.2s;
    font-family: inherit;
  }

  .btn-filtro:hover {
    border-color: #0c8cbe;
    color: #0fd6e4;
  }

  .btn-filtro.ativo {
    background: #f0883e;
    color: white;
    border-color: #f0883e;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
    gap: 16px;
  }

  .card {
    background: #161b22;
    border: 1px solid #30363d;
    border-radius: 16px;
    overflow: hidden;
    transition: all 0.3s;
  }

  .card:hover {
    border-color: #f0883e;
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(240, 136, 62, 0.1);
  }

  .card.favorito {
    border-color: #13d4ee;
    box-shadow: 0 0 12px rgba(248, 81, 73, 0.1);
  }

  .card-emoji {
    font-size: 48px;
    text-align: center;
    padding: 20px;
    background: linear-gradient(135deg, #1c2333, #161b22);
  }

  .card-body {
    padding: 16px;
  }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: start;
    margin-bottom: 10px;
  }

  .card-header h3 {
    font-size: 18px;
    color: #e6edf3;
    margin: 0;
  }

  .btn-fav {
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    transition: transform 0.2s;
    padding: 0;
  }

  .btn-fav:hover {
    transform: scale(1.3);
  }

  .tags {
    display: flex;
    gap: 6px;
    margin-bottom: 10px;
    flex-wrap: wrap;
  }

  .tag-genero {
    background: rgba(188, 140, 255, 0.15);
    color: #05f772;
    padding: 3px 10px;
    border-radius: 12px;
    font-size: 11px;
    font-weight: 600;
  }

  .tag-ano {
    background: rgba(88, 166, 255, 0.15);
    color: #58a6ff;
    padding: 3px 10px;
    border-radius: 12px;
    font-size: 11px;
    font-weight: 600;
  }

  .tag-nota {
    background: rgba(210, 153, 34, 0.15);
    color: #ee6414;
    padding: 3px 10px;
    border-radius: 12px;
    font-size: 11px;
    font-weight: 600;
  }

  .sinopse {
    color: #8b949e;
    font-size: 13px;
    line-height: 1.5;
  }

  .vazio {
    text-align: center;
    padding: 60px 20px;
    color: #484f58;
    font-size: 18px;
  }
</style>
