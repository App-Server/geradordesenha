<template>
  <div>
    <Navbar />
    <div style="padding: 2rem; text-align: center;">
      <h1>Gerador de Senhas</h1>
      <div style="margin: 1rem 0;">
        <label>
          Tamanho da Senha:
          <input
            type="number"
            v-model.number="tamanho"
            min="4"
            max="32"
            style="margin-left: 0.5rem;"
          />
        </label>
      </div>
      <div>
        <label>
          <input
            type="checkbox"
            v-model="opcoes.letrasMaiusculas"
          />
          Letras Maiúsculas
        </label>
        <label style="margin-left: 1rem;">
          <input
            type="checkbox"
            v-model="opcoes.letrasMinusculas"
          />
          Letras Minúsculas
        </label>
        <label style="margin-left: 1rem;">
          <input
            type="checkbox"
            v-model="opcoes.numeros"
          />
          Números
        </label>
        <label style="margin-left: 1rem;">
          <input
            type="checkbox"
            v-model="opcoes.simbolos"
          />
          Símbolos
        </label>
      </div>
      <br />
      <button @click="gerarSenha" class="btn btn-primary">
        Gerar Senha
      </button>
      <div v-if="senha" style="margin-top: 1rem; font-weight: bold;">
        <span style="margin-right: 1rem;">Sua Senha: <u>{{ senha }}</u></span>
        <button
          @click="copiarSenha"
          :class="['btn', copiado ? 'btn-success' : 'btn-primary', 'ml-3']"
        >
          {{ copiado ? "Copiado!" : "Copiar" }}
        </button>
      </div>
    </div>
    <div class="container mt-4">
  <h2>Por que criar senhas fortes é importante?</h2>
  <p>
    Em um mundo digital, proteger suas contas online é essencial. Senhas fortes ajudam
    a evitar ataques de hackers e garantem sua privacidade. Este gerador ajuda você a
    criar senhas seguras e difíceis de adivinhar.
  </p>
  <h3>Dicas para uma boa senha:</h3>
  <ul>
    <li>Use uma combinação de letras, números e símbolos.</li>
    <li>Nunca reutilize a mesma senha em diferentes sites.</li>
    <li>Troque suas senhas periodicamente.</li>
  </ul>
</div>

  </div>
</template>

<script>
import { ref } from "vue";
import Navbar from "./components/Navbar.vue";

const caracteres = {
  letrasMaiusculas: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
  letrasMinusculas: "abcdefghijklmnopqrstuvwxyz",
  numeros: "0123456789",
  simbolos: "!@#$%^&*()_+[]{}|;:',.<>/?`~",
};

export default {
  components: { Navbar },
  setup() {
    const tamanho = ref(12);
    const senha = ref("");
    const copiado = ref(false);
    const opcoes = ref({
      letrasMaiusculas: true,
      letrasMinusculas: true,
      numeros: true,
      simbolos: false,
    });

    const gerarSenha = () => {
      let caracteresDisponiveis = "";
      if (opcoes.value.letrasMaiusculas) caracteresDisponiveis += caracteres.letrasMaiusculas;
      if (opcoes.value.letrasMinusculas) caracteresDisponiveis += caracteres.letrasMinusculas;
      if (opcoes.value.numeros) caracteresDisponiveis += caracteres.numeros;
      if (opcoes.value.simbolos) caracteresDisponiveis += caracteres.simbolos;

      if (!caracteresDisponiveis) {
        alert("Selecione pelo menos uma opção de caractere!");
        return;
      }

      let senhaGerada = "";
      for (let i = 0; i < tamanho.value; i++) {
        const index = Math.floor(Math.random() * caracteresDisponiveis.length);
        senhaGerada += caracteresDisponiveis[index];
      }
      senha.value = senhaGerada;
      copiado.value = false;
    };

    const copiarSenha = async () => {
      try {
        await navigator.clipboard.writeText(senha.value);
        copiado.value = true;
      } catch (error) {
        console.error("Erro ao copiar senha:", error);
      }
    };

    return {
      tamanho,
      senha,
      opcoes,
      copiado,
      gerarSenha,
      copiarSenha,
    };
  },
};
</script>

<style>
/* Estilos adicionais (opcional) */
</style>
