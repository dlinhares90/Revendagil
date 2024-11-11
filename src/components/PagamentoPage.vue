<template>
  <div class="pagamento">
    <!-- Cabeçalho com a Logo da Empresa -->
    <header class="header">
      <img src="https://revendagil.com.br/wp-content/uploads/2024/10/Icone-Logo-Revendagil-sem-fundo-100x100.png" alt="Logo da RevendaGil" class="logo" />
      <h1 class="header-title">Pagamento Seguro</h1>
    </header>

    <main>
      <h2>Página de Pagamento</h2>

      <!-- Seção de Benefícios do Plano Contratado -->
      <section class="plano-info">
        <h3>Plano Contratado</h3>
        <p><strong>Plano Básico</strong></p>
        <p>Valor: <strong>R$ 39,90/mês</strong></p>
        <h4>Benefícios do Plano:</h4>
        <ul>
          <li>Acesso a conteúdos exclusivos</li>
          <li>Suporte prioritário</li>
          <li>Descontos em serviços adicionais</li>
          <li>Acesso ilimitado a novos recursos</li>
        </ul>
      </section>

      <!-- Container para as Seções lado a lado -->
      <div class="dados-pagamento-container">
        <!-- Seção de Dados da Empresa -->
        <section class="empresa">
          <h3>Dados da Empresa</h3>
          <form @submit.prevent="processarPagamento">
            <div class="form-group">
              <label for="nomeEmpresa">Nome da Empresa:</label>
              <input 
                type="text" 
                id="nomeEmpresa" 
                v-model="nomeEmpresa" 
                required 
                autocomplete="organization" 
                placeholder="Digite o nome da empresa"
              />
            </div>

            <div class="form-group">
              <label for="cnpj">CNPJ:</label>
              <input 
                type="text" 
                id="cnpj" 
                v-model="cnpj" 
                required 
                inputmode="numeric" 
                pattern="[0-9]{14}" 
                maxlength="14" 
                placeholder="Digite o CNPJ"
              />
            </div>

            <div class="form-group">
              <label for="endereco">Endereço:</label>
              <input 
                type="text" 
                id="endereco" 
                v-model="endereco" 
                required 
                autocomplete="street-address" 
                placeholder="Digite o endereço completo"
              />
            </div>

            <div class="form-group">
              <label for="email">Email:</label>
              <input 
                type="email" 
                id="email" 
                v-model="email" 
                required 
                autocomplete="email" 
                placeholder="exemplo@dominio.com"
              />
            </div>

            <div class="form-group">
              <label for="telefone">Telefone:</label>
              <input 
                type="tel" 
                id="telefone" 
                v-model="telefone" 
                required 
                inputmode="tel" 
                pattern="[0-9]{10,11}" 
                maxlength="11" 
                placeholder="Digite o telefone"
              />
            </div>
          </form>
        </section>

        <!-- Seção de Dados do Pagamento -->
        <section class="pagamento-info">
          <h3>Informações de Pagamento</h3>
          <form @submit.prevent="processarPagamento">
            <div class="form-group">
              <label for="nome">Nome completo:</label>
              <input 
                type="text" 
                id="nome" 
                v-model="nome" 
                required 
                autocomplete="name" 
                placeholder="Digite seu nome completo" 
              />
            </div>

            <div class="form-group">
              <label for="numeroCartao">Número do Cartão:</label>
              <input 
                type="text" 
                id="numeroCartao" 
                v-model="numeroCartao" 
                required 
                inputmode="numeric" 
                pattern="[0-9]{16}" 
                maxlength="16" 
                placeholder="1234 5678 9101 1121"
              />
            </div>

            <div class="form-group">
              <label for="validade">Data de Validade:</label>
              <input 
                type="month" 
                id="validade" 
                v-model="validade" 
                required 
                min="2024-01" 
              />
            </div>

            <div class="form-group">
              <label for="cvv">CVV:</label>
              <input 
                type="text" 
                id="cvv" 
                v-model="cvv" 
                required 
                inputmode="numeric" 
                pattern="[0-9]{3}" 
                maxlength="3" 
                placeholder="123"
              />
            </div>

            <button type="submit">Pagar</button>
          </form>
        </section>
      </div>
    </main>

    <!-- Rodapé com informação de segurança e desenvolvedor -->
    <footer class="footer">
      <p>Este site é seguro e desenvolvido por <strong>Hammer Consult</strong></p>
    </footer>
  </div>
</template>


<script>
export default {
  name: 'PagamentoPage',
  data() {
    return {
      nomeEmpresa: '',
      cnpj: '',
      endereco: '',
      telefone: '',
      nome: '',
      email: '',
      numeroCartao: '',
      validade: '',
      cvv: ''
    };
  },
  methods: {
    async processarPagamento() {
      // Estrutura dos dados a serem enviados
      const dadosPagamento = {
        empresa: {
          empresaNome: this.nomeEmpresa,
          empresaCnpj: this.cnpj,
          email: this.email,
          empresaEndereco: this.endereco,
          telefone: this.telefone
        },
        pagamento: {
          nome: this.nome,          
          numeroCartao: this.numeroCartao,
          validade: this.validade,
          cvv: this.cvv
        }
      };

      //console.log(this.nome);
      

      try {
        const response = await fetch('https://localhost:7202/api/v1/usuarios', { // Substitua pelo endpoint da API real
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(dadosPagamento)
          
        });

        if (response.ok) {
          const data = await response.json();
          console.log('Pagamento processado com sucesso:', data);
          alert('Pagamento realizado com sucesso!');
        } else {
          console.error('Erro ao processar pagamento:', response.statusText);
          alert('Erro ao processar pagamento.');
        }
      } catch (error) {
        console.error('Erro na requisição:', error);
        alert('Erro na requisição. Tente novamente mais tarde.');
      }
    }
  }
};
</script>


<style scoped>
.pagamento {
  max-width: 800px;
  margin: auto;
  padding: 1em;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #f9f9f9;
}

.header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
  border-bottom: 2px solid #ccc;
  padding-bottom: 10px;
}

.logo {
  width: 50px;
  height: auto;
}

.header-title {
  font-size: 1.5em;
  color: #333;
}

main h2 {
  text-align: center;
}

.plano-info {
  margin-bottom: 20px;
  padding: 1em;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #e8f5e9;
}

.dados-pagamento-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.empresa, .pagamento-info {
  flex: 1 1 45%;
  padding: 1em;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  min-width: 300px;
}

.form-group {
  margin-bottom: 1em;
}

label {
  display: block;
  margin-bottom: 0.5em;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 0.5em;
  font-size: 1em;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  width: 100%;
  padding: 0.75em;
  font-size: 1em;
  color: #fff;
  background-color: #009639;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #007a30;
}

.footer {
  text-align: center;
  padding: 1em;
  font-size: 0.9em;
  color: #666;
  border-top: 1px solid #ddd;
  margin-top: 20px;
}

.footer strong {
  color: #333;
}

@media (max-width: 768px) {
  .dados-pagamento-container {
    flex-direction: column;
  }
}
</style>


