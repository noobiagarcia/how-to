# Usando o Ambiente de HPC - Cluster Apollo

O **Cluster Apollo** possui 26 nós computacionais, os quais são equipados com processadores `Intel Xeon Skylake 5120, 14-cores, 2.2GHz` (apl01-14) e `Intel(R) Xeon(R) Gold 5320 CPU @ 2.20GHz` (apl15-26). Neste Cluster, é possível submeter Jobs utilizando os recursos do nosso ambiente de HPC. Para isso, é imprescindível possuir uma conta válida no LIneA, registrando-se como usuário ([registre-se aqui](https://register.linea.org.br/registry/co_petitions/start/coef:155)).


## Como Realizar o Login
O acesso ao nosso cluster pode ser feito através de chave `ssh`, caso ainda não tenha registrado sua chave ssh pública no nosso ambiente, entre em contato com o Service Desk por email (helpdesk@linea.org.br). Abaixo estão os comandos para acessar o ambiente do LIneA e, em seguida, o Cluster Apollo.

- **Para acessar o LIneA:**
  ```bash
    ssh SEU.USUÁRIO@login.linea.org.br
  ```
em _"YOUR.USER"_ escreva seu usuário. Após entrar no nosso ambiente execute o comando

- **Para acessar o nó de submissão:**
  ```bash
    ssh loginapl01
  ``` 
A máquina _loginapl01_ é onde você poderá fazer a alocação do nó de computação para submeter o seu job. 

!!! Atenção: Os nós de computação não possuem acesso ao seu diretório _(home)_ de usuário. Mova ou copie, para seu diretório SCRATCH, todos os arquivos necessários para a submissão do seu job.

## Como Usar a Area SCRATCH
Seu diretório SCRATCH é o local para enviar os arquivos essenciais à submissão do seu Job, assim como verificar os resultados após a execução do código. É crucial informar que **`todos os resultados e arquivos gerados devem ser transferidos de volta para o seu diretório de usuário (home)`**. Caso contrário, **`há o risco de perder esses arquivos armazenados no seu SCRATCH`**.

- **Para acessar o seu diretório SCRATCH:**
  ```bash
    ssh $SCRATCH
  ``` 
- **Para enviar arquivos para seu diretório SCRATCH:**
  ```bash
    cp <ARQUIVO> $SCRATCH
  ``` 

## Como Usar o Gerenciador de Pacotes EUPS
O [EUPS](https://github.com/RobertLuptonTheGood/eups) é um gerenciador de pacotes alternativo (e oficial do LSST) que permite carregar variáveis de ambiente e incluir o caminho para programas e bibliotecas de forma modular.

- **Para carregar o EUPS:**
  ```bash
    . /mnt/eups/linea_eups_setup.sh
  ```
- **Para listar todos os pacotes disponíveis:**
  ```bash
    eups list
  ```
- **Para listar um pacotes específico:**
  ```bash
    eups list | grep <PACOTE>
  ```
- **Para carregar um pacotes na sessão atual:**
  ```bash
    setup <NOME DO PACOTE> <VERSÃO DO PACOTE>
  ```
- **Para remover o pacote carregado:**
  ```bash
    unsetup <NOME DO PACOTE> <VERSÃO DO PACOTE>
  ```
  
## Como Submeter um Job
Um Job solicita recursos de computação e especifica os aplicativos a serem iniciados nesses recursos, juntamente com quaisquer dados/opções de entrada e diretivas de saída. O gerenciamento e agendamento das tarefas e recursos do cluster é feito através do Slurm. Logo, para submeter um Job é necessário utilizar um script como abaixo:

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                       #Name of the Partition to use
  #SBATCH --nodelist=NODE                    #Name of the Node to be allocated
  #SBATCH -J simple-job			                 #Job name
  #----------------------------------------------------------------------------#

  ##path to executable code
  EXEC=/lustre/t0/scratch/users/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```
Nesse script é preciso especificar o **nome da fila (Partition)** que será usada, o **nome do nó** que será alocado para a excecução do Job, e o **caminho para o código/programa** a ser executado. \
................................................[**_Para visualizar mais templates de script de submissão de Jobs, clique aqui_**]()...................................................

- **Para submeter o Job:**
  ```bash
    sbatch script-submit.sh
  ```
Se o script estiver correto **haverá uma saída que indica o ID do job**.

- **Para verificar o andamento e informações do Job:**
  ```bash
    scontrol show job <ID> 
   ```
- **Para cancelar o Job:**
  ```bash
    scancel <ID> 
   ```

#### Assita também os vídeos:
* How to login
* How to use EUPS
* How to use storage area
* How to submit a job

#### Alguns Comandos Slurm  
Para aprender sobre todas as opções disponíveis para cada comando, insira man <comando> enquanto estiver conectado ao ambiente do Cluster.

|Comando	| Definição|
|-----------|----------|
|sbatch	| Envia scripts de tarefas para a fila de execução|
|squeue	| Exibir estado dos jobs|
|scontrol	| Usado para exibir o estado Slurm (várias opções disponíveis apenas para root)|
|sinfo	| Exibir estado de partições e nós|
|salloc	| Envia um job para execução ou inicia um trabalho em tempo real|
