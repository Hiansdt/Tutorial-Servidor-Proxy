# Tutorial de como instalar e configurar um servidor Proxy Debian 12 com VirtualBox

### VINÍCIUS NILO HIANSDT - INB0243-3- 2022318039

**OBSERVAÇÃO: Este tutorial foi desenvolvido conforme as orientações apresentadas em sala de aula pelo professor Adamo Dal Berto, o tutorial solicitado originalmente era sobre "Servidores Padrão". No entanto, após uma conversa realizada em 10 de outubro de 2024, foi acordado que o tutorial de Servidor Proxy seria aceito em substituição ao tutorial de servidor padrão.**

### Servidores proxy:
Um servidor proxy é um intermediário que recebe solicitações de um cliente (geralmente um navegador web) e as encaminha para o servidor de destino. Ele age como um "mediador" entre o cliente e o servidor real, gerenciando o tráfego entre ambos. O proxy pode alterar ou controlar as requisições de saída, além de modificar ou filtrar as respostas recebidas antes de enviá-las de volta ao cliente.

### Funcionamento básico de um proxy:
1. Cliente solicita um recurso: O cliente (como um navegador) faz uma solicitação para acessar um recurso (por exemplo, uma página web).
2. Proxy recebe a solicitação: Ao invés de enviar a solicitação diretamente ao servidor de destino, ela é enviada ao proxy.
3. Proxy encaminha para o servidor de destino: O proxy repassa a solicitação para o servidor real, podendo modificar ou não a requisição.
4. Servidor responde ao proxy: O servidor real processa a solicitação e envia a resposta de volta ao proxy.
5. Proxy retorna a resposta ao cliente: O proxy pode processar a resposta (por exemplo, aplicar caching ou modificar o conteúdo) antes de enviá-la ao cliente final.

### Tipos de servidores proxy:
- Proxy direto: O cliente sabe explicitamente que está usando um proxy e envia as requisições através dele.
- Proxy reverso: O servidor proxy fica na frente dos servidores de destino, e os
clientes não precisam saber que ele está presente. Ele é comum em balanceamento de carga e segurança.

### Diferença entre servidor proxy e servidor "normal":
- **Servidor normal:** Um servidor normal é aquele que hospeda diretamente os recursos e serviços que os clientes acessam, como um servidor web (por exemplo, Apache ou Nginx), banco de dados ou servidor de e-mail. Ele é o ponto final da comunicação e processa diretamente as requisições dos clientes. Exemplo: Um servidor web Apache que responde diretamente às requisições de um navegador.
- **Servidor proxy:** Ao contrário do servidor normal, o proxy não é o ponto final. Ele apenas encaminha as solicitações entre o cliente e o servidor final. Seu papel é intermediar a comunicação, podendo oferecer funcionalidades como anonimato, caching, filtragem de conteúdo e controle de acesso. Para configurar seu servidor proxy, siga os passos a seguir:

Para configurar seu servidor proxy, siga os passos abaixo:

1.  Abra o Oracle VM VirtualBox:
	![[arquivo.bmp]]
2. Clique em *New* para iniciar a configuração de uma nova máquina virtual.
	![[Screenshot (182).png]]
3. Configure as informações básicas do seu servidor.
	1. Preencha os campos *Name* e *Folder* com o nome e localização do servidor em sua máquina, respectivamente.
	2. Não selecionaremos uma imagem ISO ainda, portanto deixe o campo *ISO Image* em branco
	3. Selecionaremos **Linux** em *Type* e **Debian 12 Bookworm (64-bit)** em *Version* para que tenhamos boa compatibilidade, segurança e estabilidade em nosso servidor
	![[Screenshot (190).png]]
4. Clique em *Next* para prosseguir.
5. Configurando a memória base e número de núcleos da máquina virtual
	1. Selecionaremos **2048 MB** em *Base Memory*. Isso aloca a quantidade de memória RAM que a máquina virtual poderá utilizar. No contexto de servidores, 2 GB de RAM são suficientes para um ambiente de teste ou servidores de pequeno porte. Certifique-se de não exceder a quantidade de RAM disponível no seu host, pois isso pode impactar o desempenho do sistema.
	2. Em *Processors*, configure o valor para **1 CPU**. Isso designa um núcleo do processador do seu host para a máquina virtual. Em testes e ambientes leves, um único núcleo de CPU será adequado, mas dependendo da carga de trabalho do servidor, mais núcleos podem ser alocados futuramente, se necessário.
	![[Screenshot (191).png]]
6. Clique em *Next* para prosseguir. 
7. Criando um disco rígido virtual
	1. Defina o tamanho do disco para **20 GB**. Este espaço será utilizado para armazenar o sistema operacional e outros dados. Em ambientes de produção, o tamanho do disco seria ajustado de acordo com as necessidades, mas para este tutorial, 20 GB serão suficientes para os particionamentos e testes necessários. 
	![[Screenshot (192).png]]
8. Clique em *Next* para prosseguir.
9. Após ajustar as configurações de hardware e disco rígido virtual, você será direcionado para a tela final de resumo das configurações. Nesta etapa, o Oracle VM VirtualBox exibirá um resumo das especificações da máquina virtual que você configurou. 
	1. Se as especificações estiverem corretas, clique no botão *Finish* para concluir a criação da máquina virtual. Após clicar, o Oracle VM VirtualBox criará a máquina virtual com as configurações estabelecidas. Caso tudo tenha sido configurado corretamente, você verá a nova máquina virtual listada no painel lateral esquerdo do VirtualBox.
	2. A máquina deve estar com o nome e as especificações exatas que foram definidas nos passos anteriores. 
	![[Screenshot (193).png]]
	![[{036BDBED-BB4A-4FC2-A137-C285D92E84DB}.png]]
10. Após criar a máquina virtual, o próximo passo é ajustar suas configurações para otimizar o desempenho e desativar recursos desnecessários. Para isso, clique no botão *Settings* (ou "Configurações"), indicado pelo ícone de engrenagem.
	1. Ao clicar, um modal de configurações será aberto, onde você poderá personalizar diversos aspectos da máquina virtual. É essencial que todos os passos sejam seguidos com precisão para garantir o funcionamento adequado da máquina.
		1. Desativando o Leitor de Disquetes (*Floppy*)
			1. Clique na aba *System* (ou "Sistema"). 
			2. No sub-menu *Motherboard* (ou "Placa Mãe"), localize a seção de Boot Order (Ordem de Inicialização). 
			3. Desmarque a opção *Floppy*. Isso desativa o leitor de disquetes, um componente obsoleto, liberando recursos e evitando tentativas desnecessárias de inicialização via disquete.
			![[{0EC199F8-F04E-4CD1-BF44-C6D82B4B7E23}.png]]
		2. Desativando Áudio
			1. Acesse a aba *Audio* (ou "Áudio")
			2. Desmarque a opção *Enable Audio* (ou "Habilitar Áudio"). Essa configuração desativa o áudio, que não é necessário em servidores, contribuindo para reduzir o consumo de recursos.
			![[{3B87FB64-47EC-4841-9137-0AC87DA60460} 2.png]]
		1. Configurando USB 2.0
			1. Navegue até a aba *USB*
			2. Selecione a opção *USB 2.0 (OHCI + EHCI) Controller*. Isso garante compatibilidade com dispositivos USB mais rápidos, oferecendo suporte a periféricos USB 2.0,  caso necessário.
			![[{9FEEC5A0-48AB-40F4-B208-D92BA716AD4C} 1.png]]

11. Clique em *Ok* no canto inferior direito para finalizar as configurações.
12. Após realizar todas as configurações detalhadas anteriormente, sua máquina virtual estará pronta para ser iniciada.
	1. Com a máquina selecionada, clique no botão *Start* (ou "Iniciar"), localizado no canto superior da interface do Oracle VM VirtualBox. Esse botão dará início ao processo de boot da máquina virtual. Ao clicar em *Start*, o Oracle VM VirtualBox carregará o sistema operacional configurado e iniciará a execução da máquina virtual conforme os recursos alocados. Certifique-se de que todos os ajustes de hardware e software foram configurados corretamente antes de proceder
	![[{BFB8D586-2821-465E-BA85-9AC09CD5FE06}.png]]
13. Após iniciar a máquina virtual, você verá uma tela que indica que o sistema ainda não possui um sistema operacional instalado. Antes de prosseguir, será necessário fornecer a ISO do Debian 12, que servirá como a mídia de instalação do sistema operacional na máquina virtual
	1. Se você ainda não baixou a imagem ISO do Debian 12, pode fazê-lo através do link oficial: [ https://www.debian.org/download ]
	2. Escolha a versão adequada para sua máquina virtual, no caso, a versão *64-bit (amd64)*, já que configuramos a máquina virtual para essa arquitetura anteriormente.
	![[{FAF4CAC5-59A9-4B42-BD34-429E7A04A7C9}.png]]
	
14. Após configurar a máquina virtual e confirmar que todas as etapas anteriores foram seguidas corretamente, será necessário associar a imagem ISO do Debian 12 para iniciar a instalação do sistema operacional.
	1. Feche o modal de notificações ou qualquer outra janela que tenha sido aberta após o início da máquina virtual. Na interface do Oracle VM VirtualBox, vá até o menu superior e clique em *Devices* (ou "Dispositivos").
	![[{6E45D3B0-B595-41D1-8912-757079577C2D} 1.png]]
	3. No menu suspenso que aparecer, selecione *Optical Drives* (ou "Drives Ópticos") e, em seguida, escolha *Choose a disk file...* (ou "Escolher arquivo de disco..."). 
	![[{F23936E4-78CC-45E3-86EC-CD5A59284D6A}.png]]
	5. Localize e selecione o arquivo ISO do Debian 12 que você já baixou anteriormente no seu computador. 
	![[{0E70BD18-4373-471C-8E55-42F9D8824C96}.png]]
	7. Após associar a ISO do Debian, será necessário reiniciar a máquina virtual para que a instalação do sistema operacional Debian seja iniciada: 
		1. Para reiniciar, vá até o menu superior e clique em *Machine* (ou "Máquina").
		![[{4F3ED9E3-29A6-4985-A782-4C585CB44E63}.png]]
		2. Selecione a opção *Reset* (ou "Reiniciar"). O VirtualBox solicitará uma confirmação para reiniciar a máquina. 
		![[{73DA35D3-C808-4E62-9B89-106E83B86B96}.png]]
		3. Confirme a reinicialização.
		4. A máquina virtual será reiniciada e, dessa vez, ela utilizará a ISO do Debian 12 para carregar o instalador do sistema.
15. Após reiniciar a máquina virtual, ela iniciará o processo de instalação do Debian 12 a partir da ISO que foi configurada no passo anterior. O próximo passo é dar início ao instalador e seguir com as configurações iniciais.
16. Quando a máquina virtual reiniciar, você verá a tela inicial do instalador do Debian 12. Utilize as setas direcionais para selecionar a opção *Graphical install* (ou "Instalação gráfica") e pressione Enter. 
	![[{7D1AA971-9D21-44E9-A4D5-E59D9A837FB6}.png]]
	1. Esse modo de instalação oferece uma interface gráfica intuitiva para facilitar a configuração do sistema. Ao selecionar "Graphical install", o instalador do Debian 12 será iniciado e pedirá que você configure as seguintes opções: 
		1. Language (Idioma): Selecione o idioma desejado para a instalação e para o ambiente do sistema operacional. No meu caso, foi selecionado Português (Brasil).
		![[{98D4EDC5-7E54-45E9-BB32-0133A220FBF3}.png]]
		2. Region (Região): Escolha sua região geográfica. Isso ajuda a configurar o fuso horário e outras definições regionais. No meu caso, a região foi Brasil.
		![[{58A55D5F-F5E5-4FEC-852A-C90F76628185}.png]]
		3. Keyboard Layout (Layout do Teclado): Defina o layout do teclado conforme sua preferência. O layout Português (Brasil) é geralmente utilizado no Brasil. 
		![[{113E061A-03FE-46EF-9A37-0ECA6EEA21F4}.png]]
	2. Após preencher essas opções, o instalador prosseguirá com a configuração inicial do sistema. Certifique-se de revisar cada escolha com atenção para garantir que o sistema seja configurado conforme suas necessidades.
17. Durante o processo de instalação do Debian 12, você será solicitado a configurar o nome da máquina (hostname) e o domínio. Essas configurações são cruciais para a identificação da máquina na rede e em operações de comunicação entre servidores.
	1. Quando solicitado a inserir o nome da máquina, utilize o mesmo nome que foi configurado no Oracle VM VirtualBox durante a criação da máquina virtual. O nome deve seguir o padrão, certifique-se de que o nome esteja exatamente igual ao utilizado na criação da máquina virtual, para garantir consistência e facilitar a identificação da VM
	![[{F46DD1F2-7A64-4BA7-8DC6-B5310B2BDB5F}.png]]
	3. Na etapa seguinte, será solicitado o domínio para a máquina:
		1. Insira o domínio "acme.com.br". Esse domínio será utilizado para fins de rede, servindo como parte do endereço que outros dispositivos usarão para localizar essa máquina dentro da infraestrutura, caso uma rede seja configurada posteriormente.
		![[{647E6541-2509-4C8F-AD78-E01FC76CBA66}.png]]
		
18. Na próxima etapa do processo de instalação do Debian 12, você precisará criar um usuário e definir uma senha para o usuário root. Essas configurações são essenciais para a segurança e gestão do sistema. 
	1. Quando solicitado a criar um novo usuário, insira o nome de usuário "suporte". Em seguida, será solicitado que você defina uma senha para o usuário "suporte". Para este tutorial, a senha será "aluno" 
	2. Após criar o usuário, você também será solicitado a definir uma senha para o usuário root, que é o administrador do sistema. Insira a mesma senha "aluno" que você definiu para o usuário "suporte". 
	3. O usuário root tem acesso completo ao sistema e pode realizar qualquer operação. Por isso, é fundamental garantir que apenas pessoas autorizadas conheçam a senha do root.
	   ==**Importante:** A escolha de uma senha simples como "aluno" pode ser adequada para fins educacionais, mas não é recomendada em ambientes de produção. Para sistemas em uso real, utilize senhas mais complexas que incluem uma combinação de letras maiúsculas, minúsculas, números e caracteres especiais para aumentar a segurança.==
19. Após configurar os usuários, será necessário selecionar o fuso horário. Navegue pela lista apresentada e selecione seu estado **(São Paulo é comumente escolhido como referência de fuso horário no Brasil por estar no fuso oficial de Brasília (UTC-3) e disponível para a maioria dos softwares).**
	 ![[{C69774A3-464D-4220-BF07-948673C17524} 1.png]]
20. Iniciando Particionamento de Disco
	1. Nesta etapa, você iniciará o particionamento do disco, uma fase crucial na instalação do Debian 12. A configuração correta das partições é essencial para o funcionamento adequado do sistema e para a organização dos dados. 
	2. Ao ser solicitado a escolher onde o sistema será instalado, selecione a opção "Manual" para realizar a configuração personalizada das partições.
	![[{EB960E5C-3ED6-43D7-9820-A004CB133C74} 2.png]]
	1. Em seguida, escolha o disco rígido virtual de 20 GB que você criou anteriormente. Este disco será utilizado para armazenar o sistema operacional e outros dados.
	![[{4C27B58E-150E-4811-9194-D3F955D30550}.png]]
	3. Após selecionar o disco, uma mensagem aparecerá perguntando se você deseja criar novas tabelas de partições vazias neste dispositivo. Selecione "Sim" para prosseguir. Esta ação irá permitir que você inicie o particionamento.
	![[{5823B8DA-10B9-42AA-A52B-CC5F42A7D6EE} 1.png]]
	5. Assim que a tabela de partições for criada, você verá uma representação gráfica do seu disco. Aqui, você poderá iniciar o processo de criação das partições necessárias para o funcionamento do Debian 12.
21. Agora, você irá configurar as partições do disco conforme as necessidades do sistema. A configuração recomendada é a seguinte:

|   **Ponto de Montagem**  | **Tamanho** | **Opções** |
| --- | --- | --- |
|/| 5 GB | - |
|swap| 1 GB | - |
|/home| 2 GB | nodev, nosuid, noexec |
|/var/log| 1 GB | - |
|/tmp| 1 GB | nodev, nosuid, noexec |
|/data/nginx| 11 GB | nodev, nosuid, noexec |

22. Partição **/ (Root)** 
	1. A partição root é onde o sistema operacional é instalado. É fundamental garantir que haja espaço suficiente para o sistema e suas aplicações.
23. Partição **swap**
	1. A partição swap serve como memória adicional. Em caso de esgotamento da memória RAM física, o sistema pode utilizar essa área no disco rígido, melhorando a performance em situações de carga elevada. 
24. Partição **/home**
	1. A partição home armazena os dados pessoais dos usuários.
		1. **nodev:** Impede a interpretação de dispositivos especiais neste sistema de arquivos. Isso é importante para a segurança, evitando que arquivos de dispositivo sejam criados no diretório.
		2. **nosuid:** Impede a execução de arquivos que têm a bit setuid (set user ID). Essa configuração aumenta a segurança ao impedir que usuários normais executem programas com privilégios elevados.
		3. **noexec:** Impede a execução de binários diretamente a partir dessa partição. Isso é útil para mitigar o risco de execução acidental de scripts maliciosos ou binários não autorizados.
25. Partição **/var/log** 
	1. A partição var/log armazena arquivos de log do sistema e aplicações. É crucial para auditoria e solução de problemas.
26. Partição **/tmp**
	1. A partição tmp é utilizada para armazenar arquivos temporários criados por programas. As opções configuradas aqui são semelhantes às da partição **/home** para aumentar a segurança.
27. Partição **/data/nginx**
	1. Esta partição é onde o **NGINX** armazenará seus dados, como arquivos estáticos e logs de acesso. Para uma máquina de produção, esta partição é crítica, pois o NGINX é frequentemente utilizado como um servidor web.
	2. As mesmas opções de segurança são aplicadas aqui para prevenir a execução não autorizada de arquivos.

	A escolha do tamanho de cada partição deve ser baseada nas necessidades do sistema e no uso esperado. O dimensionamento adequado é fundamental para evitar problemas de espaço no futuro.
	A configuração de opções como nodev, nosuid e noexec é uma prática recomendada em ambientes de produção para aumentar a segurança e a integridade do sistema, reduzindo o risco de execução de código malicioso. As capturas de tela abaixo ilustram o processo de seleção e configuração de algumas partições, incluindo as opções de segurança. 
	**OBS: As três primeiras partições (/, swap, home) são primárias. E todas as partições são criadas com a localização para o início**
28. Criando as partições:
	1. Selecione a partição disponível e pressione Enter
	![[{B6B8EB65-15C8-48DD-8A22-2CF61FD7628E}.png]]
	3. Selecione a opção "Criar uma nova partição"
	![[{3ACE28B9-CAE8-47A4-9EA3-FEC640CEDDFD}.png]]
	5. Defina o tamanho para a quantidade especificada na tabela acima
	![[{08A7333F-6679-4878-99B4-AA878CB74706}.png]]
	7. Defina a partição com o tipo indicado na observação acima
	![[{859B2B26-DDAF-4FBD-94C9-0B93BEA3E9DD}.png]]
	9. Selecione a opção "Início" para criar a partição no início do espaço disponível
	![[{1F50A5AA-6B18-4AB2-B8D8-D646935E568D}.png]]
	10. Selecione opção "Usar como" para selecionar o modo de uso da partição. Com a exceção da partição swap, que terá modo de uso definido como "swap", todas as partições podem ser mantidas na configuração padrão.
	![[{108575D3-AC92-47F0-9C0A-E92A6DFDC5BF}.png]]
	12. Em "Ponto de montagem", selecione o ponto de acordo com a indicação na tabela acima. Caso o ponto mencionado não esteja disponível, selecione a opção para adicionar manualmente e escreva o nome indicado na tabela.
	![[{FD3A3F2E-150B-4932-ABE9-6B285E05E098}.png]]
	![[{FD231859-522E-4916-A1FC-8C49C6E1BF69} 1.png]]
	13. Caso a partição possua opções de montagem, selecione a opção "Opções de montagem" e selecione as opções desejadas conforme indicado na tabela de partições.
	![[{1166D707-057B-48AC-A9DA-88028A3FFC56}.png]]
	![[{EB3D8B81-11D8-4D35-BE47-7D88D4041F46}.png]]
	14. Confira as configurações e selecione "Finalizar a configuração da partição"
	15. Repita o processo para todas as partições.
29. Após concluir a configuração das partições, sua tela deve refletir as configurações conforme mostrado na captura de tela abaixo.
	![[{04366F19-70B4-4E8E-93C7-42287BFCAACB}.png]]
	1. Após revisar e assegurar que todas as partições estão configuradas corretamente, será necessário confirmar as alterações no disco. Este é um passo crucial, pois as mudanças feitas serão aplicadas e não poderão ser desfeitas sem um novo particionamento.
	2. Verifique as configurações e selecione "sim".
30. Em seguida, você será solicitado a configurar o gerenciador de pacotes. 
	1. Selecione a opção "Não" para não ler mídias de instalação adicional.
	![[{44DCECC6-E58C-4D5A-B5BD-E6632113757E}.png]]
	2. Prossiga e selecione o Brasil como seu local, pois isso garantirá que os repositórios e servidores de download utilizados durante a instalação estejam otimizados para sua região.
	![[{99091914-7EB4-4C18-9D1F-114AFD31C6E5}.png]]
	3. Durante o processo de instalação, você será solicitado a escolher um espelho de repositório, ou seja, o servidor de onde o sistema irá baixar pacotes e atualizações. Selecione o espelho *debian.c3sl.ufpr.br*, que é um servidor brasileiro da Universidade Federal do Paraná (UFPR). Ele geralmente oferece boa velocidade de download para usuários no Brasil, otimizando o processo de instalação e atualizações futuras.
	![[{F359472C-F246-4317-921A-61097D63AACF} 1.png]]
	4. O instalador irá perguntar se você deseja configurar um proxy HTTP. Como este tutorial não exige o uso de um servidor proxy externo, deixe o campo de proxy HTTP vazio e clique em "Continuar" para seguir adiante. O proxy HTTP só deve ser configurado caso você tenha um servidor proxy intermediando sua conexão com a internet. Neste caso, a configuração é desnecessária.
	![[{8474E043-9B3B-4062-87FC-E486FC9609CC} 1.png]]
31. Em seguida, será oferecida a opção de participar do Popularity Contest, um programa que envia relatórios anônimos sobre pacotes instalados para os desenvolvedores do Debian. 
	1. Marque a opção "Não", pois para este tutorial e ambiente de servidor não é necessário participar deste programa.
	![[{CF0117AB-22CE-4B06-A30D-8BF9895B9B0A} 1.png]]
32. Na etapa seguinte, você será solicitado a selecionar pacotes e utilitários padrão que podem ser instalados junto com o sistema. 
	1. Desmarque todas as opções. O motivo para isso é garantir que apenas os pacotes essenciais para o funcionamento do servidor sejam instalados, mantendo o sistema enxuto e sem sobrecarga de softwares desnecessários.
	![[{52CB1B14-24D5-4EDC-9A9D-2192589D5AFA}.png]]
33. O instalador então perguntará se você deseja instalar o GRUB (Grand Unified Bootloader), que é o carregador de boot necessário para inicializar o sistema.
	1. Marque "Sim" para instalar o GRUB, pois ele é essencial para que o sistema possa ser iniciado corretamente após a instalação.
	![[{4583EDF1-DE5A-417F-B3AB-EEEC5BE14C11} 2.png]]
	2. Após a confirmação da instalação do GRUB, selecione o disco no qual o GRUB será instalado. Certifique-se de escolher o mesmo disco em que criamos as partições anteriormente, garantindo que o carregador de boot seja instalado no local correto. O disco escolhido será o responsável por iniciar o sistema sempre que a máquina virtual for ligada.
	![[{313BECF1-A646-4043-8487-2CABDFA68C3C}.png]]
34. Após a instalação do GRUB e a finalização das configurações, clique em "Continuar" para finalizar a instalação. 
	![[{8D2CA011-48D9-4190-9B9F-DC44531BAD4C}.png]]
35. O sistema reiniciará automaticamente. Assim que reiniciado, a máquina virtual estará pronta para uso com o Debian 12 e as configurações definidas.
36. Após a reinicialização do sistema, a máquina virtual exibirá um menu de boot com as opções de sistema operacional. Nesse menu, selecione a opção Debian para iniciar o sistema recém-instalado. O Debian começará a ser carregado com as configurações que fizemos anteriormente. 
37. Assim que o sistema for inicializado e a tela de login aparecer, poderá acessar as contas suporte e root no sistema.
38. Digite o nome de usuário da conta que deseja acessar.
	![[{2870FD9E-120A-4D12-8624-1ABD4F12438A} 1.png]]
39. Em seguida, digite a senha da conta de acordo com o que foi definido durante a instalação.
	![[{F18069A5-474D-461E-A124-76E30692BD7C}.png]]
40. Se a senha estiver correta, você terá acesso à conta e poderá realizar todas as operações administrativas no sistema.
	![[{8441A031-11C3-4A0C-8392-8B28EF2B2C6D}.png]]
