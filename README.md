# Simulador de Sistema de Arquivos com Journaling

###  O que é um Sistema de Arquivos
Um **sistema de arquivos** é a estrutura lógica utilizada por sistemas operacionais para organizar, armazenar e recuperar dados em dispositivos de armazenamento, como discos rígidos e SSDs. Ele define como os arquivos são nomeados, acessados e gerenciados, permitindo que usuários e aplicações interajam com os dados de forma eficiente e segura.

A importância do sistema de arquivos está em garantir:
- **Organização**: estrutura hierárquica de diretórios e arquivos.
- **Eficiência**: acesso rápido e otimizado aos dados.
- **Segurança**: controle de permissões e integridade das informações.
- **Escalabilidade**: suporte a grandes volumes de dados e múltiplos usuários.

###  Conceito de Journaling
O **Journaling** é uma técnica utilizada em sistemas de arquivos modernos para aumentar a confiabilidade e a integridade dos dados. Ele funciona como um **diário de operações**, registrando mudanças antes de aplicá-las efetivamente ao disco. Assim, em caso de falhas (como queda de energia ou travamento do sistema), é possível recuperar o estado consistente do sistema de arquivos.

#### Propósito do Journaling
- Evitar corrupção de dados em caso de falhas inesperadas.
- Permitir recuperação rápida após reinicializações.
- Garantir consistência entre metadados e dados armazenados.

#### Funcionamento
1. Antes de modificar um arquivo ou diretório, a operação é registrada no **journal**.
2. Após o registro, a operação é executada no sistema de arquivos.
3. Se ocorrer falha, o sistema usa o journal para restaurar ou completar a operação.

#### Tipos de Journaling
- **Write-Ahead Logging (WAL):** todas as operações são registradas antes de serem aplicadas, garantindo consistência total.
- **Log-Structured File System:** organiza dados e metadados em forma de log contínuo, otimizando gravações sequenciais.
- **Metadata Journaling:** registra apenas alterações nos metadados (como nomes e permissões), sendo mais rápido, mas menos completo.

---
#  Estrutura de Dados do Simulador de Sistema de Arquivos

O simulador de sistema de arquivos utiliza uma arquitetura **orientada a objetos** para representar arquivos, diretórios e toda a hierarquia do sistema.

---

## FSNode (classe abstrata)
Base para todos os elementos do sistema de arquivos (similar a um i-node simplificado).

### Atributos
- `name`: nome do arquivo ou diretório
- `parent`: referência ao diretório pai
- `createdAt` e `updatedAt`: metadados

### Métodos
- Métodos utilitários usados pelas subclasses

Todas as demais classes herdam de **FSNode**.

---

##  FSDirectory
Representa um diretório e contém outros nós.

### Atributos principais
- `children`: lista `List<FSNode>` com arquivos e diretórios

### Métodos principais
- `addChild(FSNode)`
- `removeChild(FSNode)`
- `listChildren()`
- `getChildByName(String)`
- `getType()` → retorna `"DIR"`

Modela a hierarquia do sistema de arquivos como uma **árvore**.

---

##  FSFile
Representa um arquivo comum no sistema.

### Atributos
- `content`: conteúdo textual
- `extension`: extensão do arquivo

### Métodos principais
- `read()`
- `write(String)`
- `setExtension(String)`
- `getType()` → retorna `"FILE"`

---

## FileSystemSimulator e Shell

### FileSystemSimulator
Gerencia o estado global do sistema:
- Mantém referência ao diretório raiz
- Controla a persistência do sistema (`filesystem.dat`)
- Mantém o objeto **Journal**

### Shell
Implementa o interpretador de comandos, permitindo executar:
- `mkdir`, `ls`, `rm`, `mv`, `cp`, `cat`, `echo`, etc.  
  Esses comandos manipulam diretamente os objetos **FSFile** e **FSDirectory**.

---

## 🔹 Journaling

O sistema implementa **journaling** para garantir integridade e auditoria das operações. Ele é feito pela classe **Journal**.

### Arquivo de Log
- Todas as entradas são gravadas em `.journal`
- Escrita em modo **append** (não sobrescreve nada)
- Persistente durante toda a execução

# Comandos do Shell

O **Shell** do simulador implementa um interpretador de comandos que permite manipular diretamente os objetos `FSFile` e `FSDirectory`.  
A leitura é feita linha a linha (via `Scanner` ou `BufferedReader`), e os tokens são separados com `String.split(" +")`.

---

# Comandos básicos

- ls <path>      - Listar diretório
- mkdir <path>   - Criar diretório
- touch <path>   - Criar arquivo
- cd <path>      - Mudar de diretório
- rm <path>      - Remover arquivo ou diretório
- mv <old> <new> - Renomear arquivo ou diretório
- cp <src> <dst> - Copiar arquivo
- clear          - Limpar tela
- exit           - Sair do shell

---

# Execução
- javac -d bin src/*.java
- java -cp bin Shell


# Conclusão
Este simulador demonstra como sistemas de arquivos com journaling funcionam, garantindo maior segurança e consistência dos dados. A arquitetura em Java facilita a abstração dos conceitos, permitindo explorar operações básicas de arquivos e diretórios, além de simular falhas e recuperação através do journal.

