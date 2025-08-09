import heapq
from datetime import datetime, timedelta

class SistemaAcademico:
    def __init__(self):
        self.periodo = ""
        self.materias = {}          # {nome_materia: {"topicos": [], "anotacoes": []}}
        self.topicos_gerais = {}     # {tópico: [materias_relacionadas]}
        self.avaliacoes = []         # (data, matéria, tipo, nome)
        self.anotacoes_gerais = []   # Lista de anotações livres

    # --- Configuração Inicial ---
    def perguntar_periodo(self):
        print("\n--- CONFIGURAÇÃO INICIAL ---")
        self.periodo = input("Em qual período você está? (Ex: 2023.2): ").strip()
        print(f"Período {self.periodo} registrado com sucesso!")

    # --- Cadastro de Matérias ---
    def cadastrar_materias(self):
        print("\n--- CADASTRO DE MATÉRIAS ---")
        while True:
            nome = input("\nNome da matéria (ou 'sair' para terminar): ").strip()
            if nome.lower() == 'sair':
                break
            
            if nome in self.materias:
                print(f"Matéria '{nome}' já existe!")
                continue
                
            self.materias[nome] = {"topicos": [], "anotacoes": []}
            print(f"Matéria '{nome}' cadastrada com sucesso!")
            
            # Opção para adicionar tópicos/anotações imediatamente
            self.gerenciar_conteudo_materia(nome)

    def gerenciar_conteudo_materia(self, materia):
        while True:
            print(f"\n--- Gerenciando {materia} ---")
            print("1. Adicionar tópicos")
            print("2. Adicionar anotação")
            print("3. Ver conteúdo")
            print("0. Voltar")
            
            opcao = input("Escolha: ").strip()
            
            if opcao == "1":
                self.adicionar_topicos(materia)
            elif opcao == "2":
                self.adicionar_anotacao(materia)
            elif opcao == "3":
                self.mostrar_conteudo_materia(materia)
            elif opcao == "0":
                break
            else:
                print("Opção inválida!")

    # --- Sistema de Tópicos ---
    def adicionar_topicos(self, materia):
        print(f"\n--- Adicionando tópicos a {materia} ---")
        while True:
            topico = input("Digite um tópico (ou 'sair' para terminar): ").strip()
            if topico.lower() == 'sair':
                break
                
            if topico not in self.materias[materia]["topicos"]:
                self.materias[materia]["topicos"].append(topico)
                
                # Registrar no sistema de tópicos gerais
                if topico not in self.topicos_gerais:
                    self.topicos_gerais[topico] = []
                if materia not in self.topicos_gerais[topico]:
                    self.topicos_gerais[topico].append(materia)
                
                print(f"Tópico '{topico}' adicionado!")
            else:
                print("Este tópico já existe nesta matéria!")

    def cadastrar_topico_independente(self):
        print("\n--- CADASTRO DE TÓPICOS INDEPENDENTES ---")
        while True:
            topico = input("\nDigite o tópico (ou 'sair' para terminar): ").strip()
            if topico.lower() == 'sair':
                break
            
            if topico in self.topicos_gerais:
                print("Tópico já existe! Matérias relacionadas:")
                for m in self.topicos_gerais[topico]:
                    print(f"- {m}")
            else:
                self.topicos_gerais[topico] = []
                print(f"Tópico '{topico}' criado!")
            
            print("\nDeseja vincular a alguma matéria? (s/n)")
            if input().lower() == 's':
                materia = input("Digite o nome da matéria: ").strip()
                if materia not in self.materias:
                    print("Matéria não encontrada. Criando nova...")
                    self.materias[materia] = {"topicos": [], "anotacoes": []}
                
                if topico not in self.materias[materia]["topicos"]:
                    self.materias[materia]["topicos"].append(topico)
                    self.topicos_gerais[topico].append(materia)
                    print(f"Vinculado a '{materia}'!")
                else:
                    print("Já está vinculado!")

    # --- Sistema de Anotações ---
    def adicionar_anotacao(self, materia=None):
        if materia:
            print(f"\n--- NOVA ANOTAÇÃO PARA {materia.upper()} ---")
            data = datetime.now().strftime("%d/%m/%Y %H:%M")
            texto = input("Digite sua anotação:\n")
            self.materias[materia]["anotacoes"].append({"data": data, "texto": texto})
            print("Anotação salva!")
        else:
            print("\n--- ANOTAÇÃO GERAL ---")
            data = datetime.now().strftime("%d/%m/%Y %H:%M")
            titulo = input("Título: ").strip()
            texto = input("Conteúdo:\n")
            self.anotacoes_gerais.append({"data": data, "titulo": titulo, "texto": texto})
            print("Anotação geral salva!")

    def mostrar_anotacoes(self, materia=None):
        if materia:
            print(f"\n--- ANOTAÇÕES DE {materia.upper()} ---")
            for i, anot in enumerate(self.materias[materia]["anotacoes"], 1):
                print(f"\n{i}. [{anot['data']}]")
                print(anot["texto"])
        else:
            print("\n--- ANOTAÇÕES GERAIS ---")
            for i, anot in enumerate(self.anotacoes_gerais, 1):
                print(f"\n{i}. [{anot['data']}] {anot['titulo']}")
                print(anot["texto"])

    # --- Sistema de Avaliações ---
    def cadastrar_avaliacoes(self):
        print("\n--- CADASTRO DE AVALIAÇÕES ---")
        for materia in self.materias:
            while True:
                print(f"\nMatéria: {materia}")
                tipo = input("Tipo (prova/trabalho/outro) ou 'pular': ").lower()
                if tipo == 'pular':
                    break
                
                nome = input("Nome da avaliação: ").strip()
                while True:
                    data_str = input("Data (dd/mm/aaaa): ").strip()
                    try:
                        data = datetime.strptime(data_str, "%d/%m/%Y")
                        break
                    except:
                        print("Formato inválido! Use dd/mm/aaaa")
                
                heapq.heappush(self.avaliacoes, (data, materia, tipo, nome))
                print(f"Avaliação '{nome}' agendada para {data_str}!")

    def calcular_prioridades(self):
        print("\n--- LISTA DE PRIORIDADES ---")
        if not self.avaliacoes:
            print("Nenhuma avaliação cadastrada!")
            return
        
        hoje = datetime.now()
        print("\nPróximas avaliações:")
        for i, (data, materia, tipo, nome) in enumerate(sorted(self.avaliacoes)):
            dias = (data - hoje).days
            status = "❗ URGENTE" if dias <= 3 else "⚠️ PRÓXIMO" if dias <= 7 else f"⌛ {dias} dias"
            print(f"\n{i+1}. {materia} - {nome} ({tipo})")
            print(f"   Data: {data.strftime('%d/%m/%Y')} | {status}")
            
            if dias <= 5:
                print("   Tópicos para revisar:")
                for topico in self.materias[materia]["topicos"]:
                    print(f"   - {topico}")

    # --- Sistema de Buscas ---
    def buscar_materia(self):
        print("\n--- BUSCA DE MATÉRIA ---")
        termo = input("Digite o nome ou parte: ").strip().lower()
        encontradas = [m for m in self.materias if termo in m.lower()]
        
        if not encontradas:
            print("Nenhuma matéria encontrada!")
            return
        
        print("\nMatérias encontradas:")
        for i, materia in enumerate(encontradas, 1):
            print(f"{i}. {materia}")
        
        print("\n0. Voltar")
        opcao = input("Selecione uma para detalhes: ").strip()
        if opcao.isdigit() and int(opcao) > 0:
            materia = encontradas[int(opcao)-1]
            self.mostrar_conteudo_materia(materia)
            self.gerenciar_conteudo_materia(materia)

    def buscar_topicos(self):
        print("\n--- BUSCA DE TÓPICOS ---")
        termo = input("Termo de busca: ").strip().lower()
        encontrados = False
        
        print("\nNas matérias:")
        for materia, conteudo in self.materias.items():
            matches = [t for t in conteudo["topicos"] if termo in t.lower()]
            if matches:
                encontrados = True
                print(f"\n{materia}:")
                for topico in matches:
                    print(f"- {topico}")
        
        print("\nTópicos independentes:")
        for topico, materias in self.topicos_gerais.items():
            if termo in topico.lower() and not materias:
                encontrados = True
                print(f"- {topico}")
        
        if not encontrados:
            print("Nenhum tópico encontrado.")

    # --- Visualização ---
    def mostrar_conteudo_materia(self, materia):
        print(f"\n--- CONTEÚDO DE {materia.upper()} ---")
        print("\nTópicos:")
        for i, topico in enumerate(self.materias[materia]["topicos"], 1):
            print(f"{i}. {topico}")
        
        self.mostrar_anotacoes(materia)

    def mostrar_topicos_gerais(self):
        print("\n--- TÓPICOS INDEPENDENTES ---")
        for topico, materias in self.topicos_gerais.items():
            if not materias:
                print(f"- {topico}")

    # --- Menu Principal ---
    def menu_principal(self):
        while True:
            print("\n=== SISTEMA ACADÊMICO ===")
            print(f"Período: {self.periodo}" if self.periodo else "Período: Não definido\n")
            print("1. Configurar período")
            print("2. Matérias")
            print("3. Tópicos independentes")
            print("4. Anotações gerais")
            print("5. Avaliações")
            print("6. Buscas")
            print("0. Sair")
            
            opcao = input("\nOpção: ").strip()
            
            if opcao == "1":
                self.perguntar_periodo()
            elif opcao == "2":
                self.submenu_materias()
            elif opcao == "3":
                self.submenu_topicos()
            elif opcao == "4":
                self.submenu_anotacoes()
            elif opcao == "5":
                self.submenu_avaliacoes()
            elif opcao == "6":
                self.submenu_buscas()
            elif opcao == "0":
                print("\nSistema encerrado. Bons estudos!")
                break
            else:
                print("Opção inválida!")

    def submenu_materias(self):
        while True:
            print("\n--- MATÉRIAS ---")
            print("1. Cadastrar novas")
            print("2. Listar todas")
            print("3. Gerenciar existente")
            print("0. Voltar")
            
            opcao = input("Opção: ").strip()
            
            if opcao == "1":
                self.cadastrar_materias()
            elif opcao == "2":
                print("\n--- SUAS MATÉRIAS ---")
                for i, materia in enumerate(self.materias, 1):
                    print(f"{i}. {materia}")
            elif opcao == "3":
                self.buscar_materia()
            elif opcao == "0":
                break
            else:
                print("Opção inválida!")

    def submenu_topicos(self):
        while True:
            print("\n--- TÓPICOS ---")
            print("1. Cadastrar independentes")
            print("2. Listar independentes")
            print("0. Voltar")
            
            opcao = input("Opção: ").strip()
            
            if opcao == "1":
                self.cadastrar_topico_independente()
            elif opcao == "2":
                self.mostrar_topicos_gerais()
            elif opcao == "0":
                break
            else:
                print("Opção inválida!")

    def submenu_anotacoes(self):
        while True:
            print("\n--- ANOTAÇÕES ---")
            print("1. Nova anotação geral")
            print("2. Ver anotações gerais")
            print("0. Voltar")
            
            opcao = input("Opção: ").strip()
            
            if opcao == "1":
                self.adicionar_anotacao()
            elif opcao == "2":
                self.mostrar_anotacoes()
            elif opcao == "0":
                break
            else:
                print("Opção inválida!")

    def submenu_avaliacoes(self):
        while True:
            print("\n--- AVALIAÇÕES ---")
            print("1. Cadastrar")
            print("2. Listar prioridades")
            print("0. Voltar")
            
            opcao = input("Opção: ").strip()
            
            if opcao == "1":
                self.cadastrar_avaliacoes()
            elif opcao == "2":
                self.calcular_prioridades()
            elif opcao == "0":
                break
            else:
                print("Opção inválida!")

    def submenu_buscas(self):
        while True:
            print("\n--- BUSCAS ---")
            print("1. Buscar matéria")
            print("2. Buscar tópicos")
            print("0. Voltar")
            
            opcao = input("Opção: ").strip()
            
            if opcao == "1":
                self.buscar_materia()
            elif opcao == "2":
                self.buscar_topicos()
            elif opcao == "0":
                break
            else:
                print("Opção inválida!")

# --- Execução ---
if __name__ == "__main__":
    print("=== SISTEMA DE ORGANIZAÇÃO ACADÊMICA ===")
    print("Desenvolvido para seu sucesso universitário!\n")
    
    sistema = SistemaAcademico()
    sistema.menu_principal()
