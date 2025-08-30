<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Painel de Pacientes - Hospital Monporto</title>
  <style>
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: #f0f4f8;
      margin: 0;
      padding: 0;
    }

    header {
      background: linear-gradient(135deg, #0062cc, #004085);
      color: white;
      padding: 20px;
      text-align: center;
      font-size: 22px;
      font-weight: bold;
      box-shadow: 0 3px 6px rgba(0,0,0,0.2);
    }

    header span {
      display: block;
      font-size: 16px;
      font-weight: normal;
    }

    main {
      max-width: 1100px;
      margin: 30px auto;
      padding: 20px;
    }

    .form-card {
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 3px 10px rgba(0,0,0,0.1);
      margin-bottom: 30px;
    }

    form {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      align-items: center;
    }

    input, select {
      flex: 1;
      padding: 10px;
      border-radius: 8px;
      border: 1px solid #ccc;
    }

    button {
      padding: 10px 16px;
      border: none;
      border-radius: 8px;
      background: #0062cc;
      color: white;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #004085;
    }

    .painel {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
    }

    .setor {
      background: white;
      border-radius: 12px;
      padding: 15px;
      box-shadow: 0 3px 10px rgba(0,0,0,0.1);
    }

    .setor h2 {
      margin-top: 0;
      color: #0062cc;
      font-size: 18px;
      border-bottom: 2px solid #eee;
      padding-bottom: 8px;
    }

    ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }

    li {
      background: #f8f9fa;
      margin: 6px 0;
      padding: 8px;
      border-radius: 6px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .remover {
      background: red;
      color: white;
      border: none;
      padding: 4px 8px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 12px;
    }
    .remover:hover {
      background: darkred;
    }
  </style>
</head>
<body>
  <header>
    Hospital Monporto
    <span>"Cuidando de você com excelência e dedicação"</span>
  </header>

  <main>
    <!-- Cadastro -->
    <div class="form-card">
      <h2>Cadastrar Paciente</h2>
      <form id="formCadastro">
        <input type="text" id="nomePaciente" placeholder="Nome ou Nº de atendimento" required>
        <select id="setorPaciente">
          <option value="A">Setor A</option>
          <option value="B">Setor B</option>
          <option value="UTI">UTI</option>
          <option value="Altas">Últimas Altas</option>
        </select>
        <button type="submit">Adicionar</button>
      </form>
    </div>

    <!-- Painel -->
    <div class="painel">
      <div class="setor">
        <h2>Setor A</h2>
        <ul id="listaA"></ul>
      </div>
      <div class="setor">
        <h2>Setor B</h2>
        <ul id="listaB"></ul>
      </div>
      <div class="setor">
        <h2>UTI</h2>
        <ul id="listaUTI"></ul>
      </div>
      <div class="setor">
        <h2>Últimas Altas</h2>
        <ul id="listaAltas"></ul>
      </div>
    </div>
  </main>

  <script>
    // Funções de acesso ao localStorage
    function getPacientes() {
      return JSON.parse(localStorage.getItem("pacientes")) || [];
    }

    function savePacientes(pacientes) {
      localStorage.setItem("pacientes", JSON.stringify(pacientes));
    }

    function renderListas() {
      const setores = { "A": "listaA", "B": "listaB", "UTI": "listaUTI", "Altas": "listaAltas" };
      const pacientes = getPacientes();

      // limpar listas
      Object.values(setores).forEach(id => {
        document.getElementById(id).innerHTML = "";
      });

      // popular listas
      pacientes.forEach((p, index) => {
        const li = document.createElement("li");
        li.textContent = p.nome;
        const btn = document.createElement("button");
        btn.textContent = "Remover";
        btn.classList.add("remover");
        btn.onclick = () => {
          let lista = getPacientes();
          lista.splice(index, 1);
          savePacientes(lista);
          renderListas();
        };
        li.appendChild(btn);
        document.getElementById(setores[p.setor]).appendChild(li);
      });
    }

    // Cadastro
    document.getElementById("formCadastro").addEventListener("submit", e => {
      e.preventDefault();
      const nome = document.getElementById("nomePaciente").value.trim();
      const setor = document.getElementById("setorPaciente").value;
      if (!nome) return;

      let pacientes = getPacientes();
      pacientes.push({ nome, setor });
      savePacientes(pacientes);
      renderListas();
      document.getElementById("formCadastro").reset();
    });

    // Inicializar
    renderListas();
  </script>
</body>
</html>

