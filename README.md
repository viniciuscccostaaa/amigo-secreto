// Importa o pacote readline para entrada do usuário
const readline = require("readline");

// Cria interface para entrada de dados
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

let participantes = [];

// Função para adicionar participantes
definirParticipantes = () => {
  rl.question("Digite o nome do participante (ou ENTER para finalizar): ", (nome) => {
    if (nome === "") {
      if (participantes.length < 2) {
        console.log("Adicione pelo menos dois participantes!");
        definirParticipantes();
      } else {
        realizarSorteio();
      }
    } else {
      participantes.push(nome);
      definirParticipantes();
    }
  });
};

// Função para realizar o sorteio e mostrar os pares
realizarSorteio = () => {
  let sorteio = [...participantes];
  let resultado = {};
  
  for (let participante of participantes) {
    let possiveis = sorteio.filter((p) => p !== participante);
    
    if (possiveis.length === 0) {
      console.log("Sorteio inválido. Reiniciando...");
      participantes = [];
      definirParticipantes();
      return;
    }

    let sorteado = possiveis[Math.floor(Math.random() * possiveis.length)];
    resultado[participante] = sorteado;
    sorteio = sorteio.filter((p) => p !== sorteado);
  }

  console.log("\nResultado do Amigo Secreto:");
  for (let [amigo, sorteado] of Object.entries(resultado)) {
    console.log(`${amigo} -> ${sorteado}`);
  }
  rl.close();
};

// Inicia o processo
definirParticipantes();
