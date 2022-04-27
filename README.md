# Passo a passo para criar VPC e EC2 (AWS)

## CRIANDO VPC:
1. Criamos um VPC
- É preciso definir um IPv4. Fontes para consultar:
    - https://docs.aws.amazon.com/pt_br/workspaces/latest/adminguide/a\mazon-workspaces-vpc.html 
    - CIDR RFC 1918; tabela Danilo
2. Criamos mais 3 subnets com os IPs calculados a partir do IP do VPC (usar calculadora - https://www.site24x7.com/pt/tools/ipv4-sub-rede-calculadora.html)
3. Criamos um Internet Gateway e o associamos ao VPC criado
4. Criamos uma Route Table pública e uma Route Table privada
- RT pública - conecta-se também à internet por meio do Internet Gateway
- RT privada - não se conecta ao IGW
5. Associamos nossas subnets públicas à RT pública e nossas subnets privadas à RT privada
6. Associamos o VPC a um ACL, o qual deve ter suas inbound e outbound rules permitindo todo o tráfego


## CRIANDO UM EC2:
1. Escolhemos a AMI (imagem: SO + aplicações)
2. Escolhemos a instância (configuração do hardware)
3. Em Configure Instance, selecionamos a subnet na qual nossa EC2 ficará
- Se escolhermos uma subnet pública, ela terá acesso à internet; caso seja privada, não terá. Além disso, para ter acesso, gere um IPv4 automaticamente (enable auto-assign IPv4)
4. Add Tags - identificar facilmente a instância. Exemplo: 
- Name (chave)
- ServidorWeb (valor)
5. Configure Security Group:
- Podemos criar um novo ou adicionar a um existente.
    - Source: se for 0.0.0.0./0, pode ser acessado por qualquer IP da internet - porém, apenas pessoas autenticadas pelo type (por padrão, SSH).
    - Type: se instalamos o Nginx, precisamos criar uma nova regra do tipo HTTP. Por padrão, porém, teremos também a regra SSH.
6. Ao clicar em Launch, precisamos criar uma chave SSH. 
- Para facilitar a identificação, podemos dar a ela o nome da nossa instância.
7. Fazemos download da chave (.pem) e damos launch. 

## CONECTANDO-SE À INSTÂNCIA PELA SSH - CHAVE PEM
1. Abrimos nosso terminal
2. Localizamos a pasta na qual está a chave
3. Para que a chave não possa ser alterada e, sim, só possa ser lida, executamos chmod 400 nomeDoArquivoChave.extensao. No caso, temos chmod 400 ServidorWeb.pem
4. Conectar à instância usando a DNS pública:
ssh -i “nomeDaChave.extensao” ubuntu@ec2 … ⇒ Sempre pegar no Conectar-se à instância > Cliente SSH!  
No caso de ter se conectado por uma SSH criada no próprio computador, o arquivo a ser enviado é o .pub.
Agora, estamos dentro do nosso Linux, podendo usar todos os comandos Linux normalmente.

