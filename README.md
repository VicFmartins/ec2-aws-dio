# ec2-aws-dio

# Gerenciamento de Instâncias EC2 na AWS

## Anotações e Insights sobre EC2

### O que é EC2?

### Principais Conceitos

### Gerenciamento de Instâncias

### Boas Práticas e Segurança

## Conclusão





### O que é EC2?

Amazon Elastic Compute Cloud (Amazon EC2) é um serviço web que fornece capacidade computacional segura e redimensionável na nuvem. Ele foi projetado para tornar a computação em nuvem mais fácil para os desenvolvedores. O EC2 permite que você obtenha e configure a capacidade de computação com requisitos mínimos de atrito. Ele fornece controle completo sobre seus recursos de computação e é executado no ambiente de computação comprovado da Amazon. O Amazon EC2 reduz o tempo necessário para obter e inicializar novas instâncias de servidor para minutos, permitindo que você escale a capacidade para cima ou para baixo rapidamente conforme suas necessidades de computação mudam. Ele oferece um modelo de pagamento conforme o uso, o que significa que você paga apenas pela capacidade que realmente usa, sem a necessidade de investimentos iniciais em hardware.





### Principais Conceitos

Para entender e utilizar o Amazon EC2 de forma eficaz, é fundamental familiarizar-se com alguns conceitos chave:

*   **Instâncias:** Uma instância é um servidor virtual na nuvem. Você pode configurar instâncias com diferentes sistemas operacionais (Linux, Windows, etc.), capacidades de CPU, memória e armazenamento. Cada instância é lançada a partir de uma Amazon Machine Image (AMI).

*   **Amazon Machine Images (AMIs):** Uma AMI é um modelo pré-configurado que contém o sistema operacional, o servidor de aplicativos e os aplicativos necessários para lançar uma instância. Você pode usar AMIs fornecidas pela AWS, pela comunidade, pelo AWS Marketplace ou criar suas próprias AMIs personalizadas.

*   **Tipos de Instância:** Os tipos de instância são classificados por famílias (uso geral, otimizadas para computação, otimizadas para memória, otimizadas para armazenamento, aceleradas por GPU) e tamanhos (nano, micro, small, medium, large, etc.). A escolha do tipo de instância depende dos requisitos de desempenho e custo da sua aplicação.

*   **Pares de Chaves (Key Pairs):** Pares de chaves são credenciais de segurança que você usa para provar sua identidade ao se conectar a uma instância. Um par de chaves consiste em uma chave pública (armazenada na AWS) e uma chave privada (armazenada por você). A chave privada é necessária para se conectar via SSH (para Linux) ou RDP (para Windows).

*   **Volumes Amazon EBS (Elastic Block Store):** O EBS fornece volumes de armazenamento em bloco persistentes para uso com instâncias EC2. Os volumes EBS são como discos rígidos virtuais que podem ser anexados a uma instância. Eles são independentes da vida útil da instância, o que significa que os dados persistem mesmo após a instância ser encerrada.

*   **Grupos de Segurança (Security Groups):** Um grupo de segurança atua como um firewall virtual para suas instâncias, controlando o tráfego de entrada e saída. Você pode definir regras para permitir ou negar tráfego com base em protocolos, portas e endereços IP de origem/destino. É uma camada essencial de segurança para suas instâncias EC2.

*   **Endereços IP Elásticos (Elastic IP Addresses - EIPs):** Um EIP é um endereço IP público estático que você pode alocar para sua conta AWS e associar a uma instância EC2. Diferente dos IPs públicos dinâmicos que mudam a cada reinicialização da instância, um EIP permanece o mesmo, sendo ideal para aplicações que exigem um endereço IP fixo.

*   **Regiões e Zonas de Disponibilidade (Regions and Availability Zones):** A AWS opera em várias regiões geográficas ao redor do mundo. Cada região é composta por várias Zonas de Disponibilidade isoladas e fisicamente separadas. Lançar instâncias em diferentes Zonas de Disponibilidade dentro de uma região aumenta a tolerância a falhas e a alta disponibilidade da sua aplicação.





### Gerenciamento de Instâncias

O gerenciamento de instâncias EC2 envolve diversas operações essenciais para o ciclo de vida de suas aplicações na nuvem. Compreender e dominar essas operações é crucial para manter a disponibilidade, segurança e eficiência de seus recursos.

*   **Lançamento de Instâncias:** O processo de lançamento de uma instância envolve a seleção de uma AMI, um tipo de instância, a configuração de detalhes da instância (rede, armazenamento, grupos de segurança) e a associação de um par de chaves. É o primeiro passo para colocar sua aplicação no ar.

*   **Conexão com Instâncias:** Após o lançamento, é necessário conectar-se à instância para configurá-la. Para instâncias Linux, a conexão é feita via SSH usando a chave privada do par de chaves. Para instâncias Windows, a conexão é feita via RDP, e a senha do administrador é descriptografada usando a chave privada.

*   **Parar, Iniciar e Reiniciar Instâncias:**
    *   **Parar:** Desliga a instância. Os dados no volume EBS persistem, mas os dados no armazenamento de instância efêmero são perdidos. O IP público dinâmico é liberado. Você não é cobrado pelo tempo de computação enquanto a instância está parada, apenas pelo armazenamento EBS.
    *   **Iniciar:** Liga uma instância parada. Uma nova instância é iniciada, e um novo IP público dinâmico é atribuído (a menos que um EIP esteja associado).
    *   **Reiniciar:** Reinicia a instância. É similar a um reboot de um servidor físico. Os dados e o IP público dinâmico são mantidos.

*   **Encerrar Instâncias:** A ação de encerrar uma instância a desliga permanentemente e libera todos os recursos associados (exceto volumes EBS que não foram configurados para serem excluídos no encerramento). Após o encerramento, a instância não pode ser iniciada novamente. É importante fazer backup de quaisquer dados importantes antes de encerrar uma instância.

*   **Monitoramento:** O Amazon CloudWatch é o serviço de monitoramento padrão para instâncias EC2. Ele coleta e processa dados brutos do EC2 em métricas legíveis e quase em tempo real. Você pode usar o CloudWatch para monitorar a utilização da CPU, uso da rede, status da instância e criar alarmes para notificar sobre limites excedidos.

*   **Escalabilidade (Auto Scaling):** O Auto Scaling permite que você configure grupos de instâncias EC2 para escalar automaticamente para cima ou para baixo em resposta à demanda. Isso garante que sua aplicação tenha a capacidade necessária para lidar com picos de tráfego e economiza custos ao reduzir a capacidade quando a demanda é baixa.





### Boas Práticas e Segurança

Ao trabalhar com instâncias EC2, a implementação de boas práticas e a atenção à segurança são fundamentais para proteger seus dados e garantir a disponibilidade de suas aplicações. Algumas das principais considerações incluem:

*   **Princípio do Menor Privilégio:** Conceda apenas as permissões necessárias para que suas instâncias e usuários executem suas funções. Evite usar credenciais de root e utilize perfis IAM (Identity and Access Management) com políticas de permissão bem definidas.

*   **Configuração de Grupos de Segurança:** Configure grupos de segurança de forma restritiva, permitindo apenas o tráfego essencial para suas aplicações. Por exemplo, se sua aplicação web roda na porta 80 (HTTP) e 443 (HTTPS), permita apenas essas portas do tráfego de entrada da internet (0.0.0.0/0). Para acesso SSH, restrinja o acesso apenas a IPs conhecidos e confiáveis.

*   **Uso de Pares de Chaves Seguros:** Proteja sua chave privada. Nunca a compartilhe e armazene-a em um local seguro. Se a chave privada for comprometida, qualquer pessoa com acesso a ela poderá se conectar às suas instâncias.

*   **Atualizações e Patches:** Mantenha o sistema operacional e os softwares instalados em suas instâncias EC2 sempre atualizados com os patches de segurança mais recentes. Isso ajuda a proteger contra vulnerabilidades conhecidas.

*   **Backup e Recuperação de Desastres:** Implemente uma estratégia robusta de backup para seus volumes EBS. Utilize snapshots para criar cópias pontuais de seus dados. Considere também a replicação de dados entre regiões para cenários de recuperação de desastres.

*   **Monitoramento Contínuo:** Utilize o Amazon CloudWatch para monitorar o desempenho e a segurança de suas instâncias. Configure alarmes para atividades suspeitas, uso excessivo de recursos ou falhas de sistema. O AWS CloudTrail pode ser usado para auditar as chamadas de API feitas em sua conta, ajudando a identificar atividades não autorizadas.

*   **Criptografia de Dados:** Criptografe seus volumes EBS e snapshots para proteger os dados em repouso. A AWS KMS (Key Management Service) pode ser utilizada para gerenciar as chaves de criptografia.

*   **Remoção de Recursos Não Utilizados:** Encerre instâncias, volumes EBS e outros recursos que não são mais necessários. Isso não apenas reduz custos, mas também diminui a superfície de ataque da sua infraestrutura.



