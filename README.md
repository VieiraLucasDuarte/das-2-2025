DAS 2
Lucas Duarte Vieira

27/02/2025

Trade-offs: Decisão sobre as ferramentas necessárias para um projeto, considerando vantagens e desvantagens de cada escolha.
Exemplos:
Durabilidade: Dados duráveis garantem que as informações permaneçam por mais tempo. Características de sistemas transacionais ajudam nisso.
Cache: Memória temporária que acelera a busca de dados, armazenando informações para acesso mais rápido.
Escalabilidade: Garantir que uma aplicação suporte altas demandas.
Exemplos:
Servidor Local: Maior rentabilidade, com gerenciamento fixo.
Servidor Cloud: Maior segurança e flexibilidade para escalar a aplicação.
Automatização: Automatizar processos e recursos do sistema.
Exemplos:
Aplicação que monitora servidores e gera alertas.
Sistema que recria máquinas virtuais sozinho.
Sistema que aplica recursos automaticamente para manter a aplicação online.
IaC (Infraestrutura como Código): Gestão da infraestrutura através de código.
Reduz erros por configurações manuais.
Propaga mudanças de forma uniforme e ágil.
Facilita a execução e manutenção de aplicações.
Recursos Descartáveis: Tratar recursos como substituíveis.
Recursos podem ser facilmente trocados e atualizados automaticamente, devido à sua descartabilidade.

06/03

Automatizar o provisionamento das máquinas e implementar infraestrutura como código. Não tratar os dados como algo único em todas as situações; é importante sempre manter opções de backup para restaurar o sistema de forma rápida. Sistemas fortemente acoplados, aqueles que não podem ser substituídos facilmente por outros, devem ser tratados com cautela. No caso de bancos de dados, frequentemente encontramos sistemas altamente acoplados, mas é possível usar soluções como o ELB (Elastic Load Balancer), distribuindo a carga entre várias réplicas do banco. Sempre garanta redundância, inclusive no ELB, para evitar falhas no sistema.
Ao escolher o banco de dados adequado, devemos considerar dois grandes paradigmas: o relacional tradicional, que apresenta limitações de escalabilidade horizontal. Criar réplicas do banco pode ajudar, mas, geralmente, as réplicas são apenas para leitura, o que limita a escalabilidade e pode tornar o banco mais lento.
Por outro lado, bancos NoSQL são projetados para escalabilidade horizontal, oferecendo alta performance e flexibilidade, especialmente quando não é possível modelar bem os dados em tabelas tradicionais.
Na AWS, o objetivo é otimizar os custos, garantindo que o cliente pague apenas pelo que realmente utiliza, sem criar soluções complicadas para tarefas simples.
Utilize cache para minimizar a quantidade de requisições de dados, melhorando a performance. Em termos de segurança, a AWS oferece serviços gerenciados (como o S3), que cuidam da responsabilidade pela segurança. Muitas empresas, porém, não têm uma segurança robusta, confiando apenas em firewalls e antivírus, sem entender a importância do controle adequado. Sempre siga o princípio do privilégio mínimo, garantindo que cada usuário tenha apenas os acessos necessários, nada mais.
A infraestrutura global da AWS conta com 36 regiões no mundo, 114 zonas de disponibilidade e 700 pontos de presença. É importante considerar as regiões que os clientes precisam acessar; quanto mais próxima a região, mais rápida será a conexão. A rede entre as zonas de disponibilidade é totalmente redundante e utiliza a própria rede privada da AWS. Além disso, as Local Zones permitem que os serviços sejam executados em áreas onde não há regiões da AWS, por meio de pequenas AZs (zonas de disponibilidade) em locais específicos.
O Wavelength, por sua vez, permite executar serviços da AWS em operadoras de telefonia, evitando congestionamentos nas redes tradicionais de internet, aproveitando a proximidade com as redes 5G.

10/03

Todas as regiões da AWS possuem, no mínimo, três zonas de disponibilidade (AZs), embora algumas regiões tenham mais. As AZs são basicamente data centers.
As AZs servem para garantir alta disponibilidade. Já as Local Zones têm uma estrutura semelhante a uma AZ, mas são menores e não exigem a criação de uma nova região (por exemplo, a Argentina possui uma Local Zone em Buenos Aires). Elas são usadas para oferecer serviços computacionais e conteúdos de forma mais rápida. Um exemplo de uso é o streaming de spam da Netflix, que utiliza Local Zones.
Já as Edge Locations são mais de 700 pontos espalhados pelo mundo. Elas atuam como caches regionais intermediários, buscando informações da região principal para agilizar o acesso.
Segurança de Acesso
O modelo de responsabilidade na nuvem é de responsabilidade compartilhada:
Quando você executa sua própria infraestrutura (como servidores físicos), você é totalmente responsável pela segurança.
Ao usar serviços da AWS (como IaaS/EC2), a responsabilidade é compartilhada: a AWS cuida da infraestrutura (como as regiões, zonas de disponibilidade e Edge Locations), e você é responsável pela configuração e segurança dos seus dados e aplicações.
Por exemplo, no S3 (serviço do tipo PaaS - Plataforma como Serviço), ao criar um bucket, você recebe um endereço para armazenar arquivos, parecido com o funcionamento de um Dropbox. A infraestrutura (servidores, rede, firewall) é gerenciada pela AWS, mas proteger os dados armazenados é sua responsabilidade.
Princípios Fundamentais de Segurança
Gerenciamento de Identidade: Utilize um provedor de identidade confiável (não armazene usuários e senhas diretamente no banco de dados). Use ferramentas como o Keycloak para gerenciar credenciais de forma segura.
Proteção de Dados em Trânsito e em Repouso: Utilize SSL, TLS e HTTPS para criptografar dados durante a transmissão. Para dados em repouso (armazenados em disco, como no S3 ou no HD dos servidores), também aplique criptografia.
Segurança em Todas as Camadas: Implante antivírus, firewalls e outros mecanismos de proteção em todos os níveis da infraestrutura.
Proteção de Dados Sensíveis: Limite o acesso direto ao banco de dados. O acesso deve passar obrigatoriamente por sistemas de autenticação.
Rastreabilidade: Garanta que seja possível saber quando um usuário acessou o sistema e o que ele fez. Mantenha registros de auditoria para atividades na nuvem.
Preparo para Incidentes: Tenha uma equipe de segurança dedicada a monitorar, identificar e corrigir problemas de segurança.
Automação de Boas Práticas: Automatize os processos de segurança sempre que possível para reduzir o risco de erro humano e minimizar brechas.
Conceitos de Nuvem: Autenticação e Autorização
Autenticação: Provar que o usuário é quem diz ser. Isso pode ser feito com autenticação de dois fatores (senha + biometria ou gerador de senhas, como apps no celular).
Autorização: Definir o que o usuário pode ou não fazer no sistema (por exemplo, permitir acesso a buckets S3, mas não ao banco de dados). Isso é controlado pelo IAM (Identity and Access Management) da AWS, aplicando o princípio do privilégio mínimo — o usuário só deve ter as permissões estritamente necessárias para suas funções.
Criptografia entre Contas: Sempre utilize criptografia para proteger dados transferidos entre contas diferentes.

13/03

Modelo de Responsabilidade Compartilhada
Na nuvem, a segurança é uma responsabilidade dividida entre a AWS e o cliente. A AWS é responsável pela segurança da infraestrutura global (servidores, rede, data centers), enquanto o cliente é responsável pela segurança do que ele implementa na nuvem (aplicações, dados, configurações).
Princípio do Privilégio Mínimo
O acesso dos usuários deve ser o menor possível para que eles realizem suas tarefas. Em outras palavras, conceda apenas as permissões estritamente necessárias para cada função. Isso reduz o risco de ações acidentais ou maliciosas.
Autenticação e Autorização
Autenticação: Processo de confirmar que o usuário é quem ele afirma ser (por exemplo, através de login e senha ou autenticação de dois fatores).
Autorização: Define o que o usuário pode ou não fazer dentro do ambiente. Ou seja, quais recursos ele pode acessar e que tipo de ações ele pode executar.
Identity and Access Management (IAM)
O IAM é o serviço da AWS que gerencia identidades e permissões de acesso. Ele permite o controle de:
Usuários: Representam pessoas ou aplicações que precisam de acesso. Um usuário tem credenciais (nome de usuário, senha, chaves de acesso) e pode se autenticar via console (interface web) ou acesso programático (API/CLI).
Grupos de Usuários: Conjunto de usuários que compartilham as mesmas permissões.
Roles (Funções): Permissões temporárias que podem ser atribuídas a usuários ou serviços para acessar recursos específicos. Roles permitem mudar dinamicamente o que pode ser acessado, sem a necessidade de criar novas credenciais.
Policies (Políticas): Conjunto de regras que definem permissões.

17/03

RBAC (Role-Based Access Control)
O RBAC é um modelo de controle de acesso baseado em funções. Ele organiza permissões conforme os papéis atribuídos a usuários, como acontece em bancos de dados.
Policies de Identidade: São regras definidas para usuários ou grupos dentro do IAM, geralmente configuradas pelo usuário root. Elas determinam o que um usuário pode ou não fazer.
Policies de Recurso: São permissões aplicadas diretamente aos recursos, como um bucket no S3. Elas definem quem pode acessar e o que pode ser feito com aquele recurso específico.
ARN (Amazon Resource Name)
O ARN é um identificador único para cada recurso na AWS. Ele é utilizado para garantir que qualquer usuário ou serviço tenha acesso correto e seguro ao seu recurso (por exemplo, um bucket S3).
Ao criar uma política, o ARN é usado para especificar exatamente qual recurso será protegido e quem terá permissão (o Principal identifica o usuário ou serviço autorizado).
Tipos de Armazenamento
Armazenamento em Blocos (EBS):
É um tipo de armazenamento que organiza os dados em blocos e permite o acesso a partes específicas dos arquivos, ideal para discos de servidores.
Sistemas de Arquivos em Rede (EFS):
Funciona como uma rede de compartilhamento de arquivos entre diferentes servidores, permitindo que múltiplas instâncias acessem os mesmos dados de forma simultânea.
Armazenamento de Objetos (S3):
Utiliza um modelo onde os arquivos são armazenados como objetos que incluem os dados propriamente ditos e seus metadados (informações sobre o arquivo, como dono, data de criação, etc.).
O S3 é altamente escalável, sem limite prático para o número de arquivos armazenados. Cada objeto pode ter até 5 TB e recebe uma URL única e global para acesso.

24/03

Quem trabalha com o Amazon S3 precisa se preocupar com o gerenciamento de espaço de armazenamento. Apesar do S3 ser altamente escalável, armazenar grandes volumes de dados impacta diretamente nos custos.
Para economizar, existem algumas práticas importantes:
Gerenciamento de Ciclo de Vida (Lifecycle):
Permite criar regras automáticas para apagar arquivos que não são mais necessários ou mover objetos para classes de armazenamento mais baratas conforme o tempo passa.
Classes de Armazenamento:
O S3 oferece várias classes (como Standard, Infrequent Access, Glacier) que têm custos diferentes. É possível mover arquivos automaticamente entre essas classes para reduzir despesas.
Regras de Expiração:
Empresas podem criar políticas para deletar arquivos após determinado tempo, conforme a necessidade do negócio.
Versionamento no S3
Por padrão, o versionamento dos buckets no S3 vem desativado.
Se você ativar o versionamento, não é possível desativá-lo, apenas suspender temporariamente.
Cada versão de um arquivo é armazenada como uma cópia completa do objeto original.
O S3 não permite edição direta de arquivos; para alterar algo, é necessário fazer o upload de uma nova versão.
O que é CORS no S3?
CORS (Cross-Origin Resource Sharing) é um mecanismo de segurança que controla quem pode acessar os seus arquivos do S3 a partir de outros domínios.
Quando ativado, o CORS permite que aplicações hospedadas em domínios diferentes possam buscar ou consumir os seus dados de maneira segura, impedindo acessos não autorizados.

27/03

Proteção e Acesso aos Buckets S3
É muito importante evitar expor seus buckets diretamente para a internet. Para proteger seus dados, é recomendado:
Usar Criptografia (SSE-S3):
A criptografia Server-Side Encryption (SSE-S3) utiliza um processo de "envelopamento", onde as chaves de criptografia são geradas e gerenciadas automaticamente pela AWS.
Privacidade por Padrão:
Quando um bucket é criado, ele vem completamente privado. Apenas o proprietário tem acesso.
Você pode:
Permitir acesso controlado: Definindo quais usuários ou serviços podem acessar o bucket.
Tornar o bucket público: Por exemplo, para hospedar um site estático, mas isso deve ser feito com muito cuidado.
Batch Operations no S3
O Batch Operations é uma funcionalidade que facilita a execução de ações em larga escala dentro do S3.
Com ele, você pode:
Copiar ou mover vários arquivos para outro bucket.
Selecionar especificamente quais arquivos quer processar.
Evitar problemas de lentidão ao listar muitos arquivos de uma só vez, já que em buckets com milhões de objetos isso pode ser muito demorado.

03/04

EC2 - Computação em Nuvem
Na AWS, existem várias maneiras de rodar recursos na nuvem. Um dos principais serviços para isso é o EC2 (Elastic Compute Cloud), o segundo serviço lançado pela AWS.
O EC2 permite criar servidores virtuais na nuvem. Esses servidores não se comunicam diretamente com o hardware físico — eles se conectam a um hypervisor, que gerencia o acesso aos recursos como CPU, memória e armazenamento. A comunicação com a internet, outros servidores e serviços da AWS (como o S3) é feita através de placas de rede virtuais.
Para armazenamento, o EC2 normalmente usa o EBS (Elastic Block Store).
Existem dois tipos principais de armazenamento:
Ephemeral: armazenamento temporário que desaparece ao desligar a máquina.
Persistente: permanece disponível mesmo após desligar a instância.
Como Criar um Servidor EC2
O processo envolve alguns passos:
Escolher a AMI (Amazon Machine Image):
A AMI define o sistema operacional (Linux, Windows, macOS) e torna o ambiente repetível, reutilizável e recuperável.
A AMI é vinculada a uma região, mas pode ser copiada para outra.
Existem opções:
Imagens AWS Oficiais: testadas pela AWS.
AWS Marketplace: imagens prontas de terceiros (podem incluir softwares adicionais).
Community AMIs: imagens públicas, sem garantias da AWS (use por sua conta e risco).
Lançar a Instância:
Ao criar a máquina (launch), ela passa para o estado:
Pending → enquanto inicializa.
Running → quando está em funcionamento.
Você também pode:
Reiniciar (reboot) a instância.
Parar (stop) a instância.
Terminar (terminate) — apaga a instância permanentemente.
Hibernar — salva o estado atual da máquina.
Escolhendo a Instância EC2
O nome da instância segue um padrão:
Primeira letra: identifica a família (tipo de uso).
Número: a geração (versão).
Sufixos: indicam características específicas (por exemplo, "g" ou "gn" para instâncias Graviton baseadas em ARM).
Exemplo: t3.micro, c6g.large.
Além disso, o Compute Optimizer é uma ferramenta da AWS que ajuda a verificar se suas instâncias estão:
Overprovisioned: recursos sobrando.
Underprovisioned: recursos faltando.
Armazenamento Adicional
EBS: Armazenamento em bloco para EC2 (não é file share).
S3: Armazenamento de objetos (não é um sistema de arquivos).
EFS (Elastic File System): File share para sistemas Linux. Usa o protocolo NFSv4 e você paga apenas pelo armazenamento utilizado.
FSx: File share que pode ser usado com Windows, pois suporta protocolos como SMB.

07/04

EBS hd do servidor

As regiões da AWS são compostas por zonas de disponibilidade (AZs), geralmente pelo menos três por região.
HPC (High Performance Computing) é uma abordagem em que servidores são organizados em clusters, ficando fisicamente próximos (às vezes no mesmo rack ou até na mesma máquina), com o objetivo de minimizar a latência e acelerar o processamento de cargas pesadas.
Alta disponibilidade com Spread significa distribuir as instâncias entre múltiplas AZs para garantir tolerância a falhas, o que aumenta a resiliência mas pode gerar mais latência.
Partitionamento é usado por sistemas como Apache Kafka, Cassandra e Spark, que realizam processamento de dados em larga escala ou em tempo real.
Sharding significa dividir os dados entre vários servidores — esses servidores ficam próximos uns dos outros para manter a performance. É uma solução intermediária entre um cluster e o spread, ideal para ferramentas distribuídas.
Modelos de instância EC2:
On-demand: paga-se somente pelo tempo de uso, sem compromissos.
Reserved Instances: compromete-se com o uso específico de instâncias por um período determinado. Oferece grandes descontos, especialmente com pagamento antecipado.
Savings Plans: modelo flexível de compromisso com uso computacional em troca de descontos, sem travar em uma instância específica.
Spot Instances: você compra capacidade ociosa com grande desconto, mas a AWS pode interromper essas instâncias com pouco aviso, normalmente com 2 minutos de antecedência.
Segurança:
A proteção das máquinas não deve depender de intervenção manual. As medidas de segurança devem ser automatizadas para garantir a consistência e a confiabilidade.

10/04

BANCO DE DADOS – Escalabilidade e Modelagem
Escalar um banco de dados é sempre um desafio. É preciso considerar quanto armazenamento será necessário, qual estrutura de dados será adotada e qual nível de latência é aceitável. A solução precisa ser flexível o suficiente para lidar com diferentes modelos de dados.
No caso dos bancos relacionais, que nem sempre têm suporte completo para escalabilidade nativa na nuvem, a AWS oferece o RDS (Relational Database Service), um serviço gerenciado para implantar bancos como MySQL, PostgreSQL, Oracle, SQL Server, entre outros.
Se preferir, é possível criar uma instância EC2 (máquina virtual) e instalar o banco de dados manualmente, o que seria uma abordagem não gerenciada. Porém, o uso de serviços gerenciados simplifica bastante a manutenção, backups e escalabilidade.
No universo não relacional, a AWS oferece o DynamoDB, um banco NoSQL altamente escalável, usado inclusive pela própria amazon.com. Outros exemplos de bancos especializados incluem:
Amazon Neptune: banco orientado a grafos.
Serviços de blockchain: voltados para registros descentralizados.
Amazon ElastiCache: armazena cópias dos dados em memória para respostas muito rápidas, ideal para caching.
Tipos de escalabilidade:
Escalabilidade vertical: aumentar os recursos (CPU, memória, etc.) de um único servidor.
Escalabilidade horizontal: criar réplicas do banco de dados, distribuindo a carga entre várias instâncias.


