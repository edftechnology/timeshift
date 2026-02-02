# Como configurar/instalar/usar o `timeshift` no `Linux Ubuntu`

## Resumo

Neste documento estão contidos os principais comandos e configurações para configurar/instalar/usar o `timeshift` no `Linux Ubuntu`.

## _Abstract_

_This document contains the main commands and settings for configuring/installing/using the `timeshift` on `Linux Ubuntu`._


## Descrição [2]

### `timeshift`

`TimeShift` é uma aplicação para sistemas `Linux` que oferece uma maneira simples e eficaz de fazer _backups_ e restaurar o sistema para um estado anterior em caso de falhas ou erros. Ele cria _snapshots_ do sistema em intervalos regulares, permitindo a recuperação rápida e fácil do sistema para um estado estável anterior. Com uma interface amigável, os usuários podem agendar _backups_ automáticos e gerenciar facilmente os _snapshots_.

## 1. Como configurar/instalar/usar o `timeshift` no `Linux Ubuntu` [1][3]

Para configurar/instalar/usar o `timeshift` no `Linux Ubuntu`, você pode seguir estes passos:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando: `Ctrl + Alt + T`

2. Certifique-se de que seu sistema esteja limpo e atualizado.

    2.1 Limpar o `cache` do gerenciador de pacotes `apt`. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo `apt` e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando: `sudo apt clean` 
    
    2.2 Remover pacotes `.deb` antigos ou duplicados do cache local. É útil para liberar espaço, pois remove apenas os pacotes que não podem mais ser baixados (ou seja, versões antigas de pacotes que foram atualizados). Digite o seguinte comando: `sudo apt autoclean`

    2.3 Remover pacotes que foram automaticamente instalados para satisfazer as dependências de outros pacotes e que não são mais necessários. Digite o seguinte comando: `sudo apt autoremove -y`

    2.4 Buscar as atualizações disponíveis para os pacotes que estão instalados em seu sistema. Digite o seguinte comando e pressione `Enter`: `sudo apt update`

    2.5 **Corrigir pacotes quebrados**: Isso atualizará a lista de pacotes disponíveis e tentará corrigir pacotes quebrados ou com dependências ausentes: `sudo apt --fix-broken install`

    2.6 Limpar o `cache` do gerenciador de pacotes `apt`. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo `apt` e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando: `sudo apt clean` 
    
    2.7 Para ver a lista de pacotes a serem atualizados, digite o seguinte comando e pressione `Enter`:  `sudo apt list --upgradable`

    2.8 Realmente atualizar os pacotes instalados para as suas versões mais recentes, com base na última vez que você executou `sudo apt update`. Digite o seguinte comando e pressione `Enter`: `sudo apt full-upgrade -y`
    

Para configurar/instalar/usar o `Timeshift` no `Linux Ubuntu` através do `Terminal Emulator`, você pode seguir os seguintes passos:

1. **Instalar o `btrfs-tools` no `Linux Ubuntu`, você pode usar o seguinte comando no Terminal**: `sudo apt install btrfs-progs -y`
    
    O pacote `btrfs-progs` contém as ferramentas de gerenciamento do sistema de arquivos `BTRFS`. Note que `btrfs-tools` é um nome de pacote mais antigo; em versões mais recentes do `Ubuntu`, ele foi renomeado para `btrfs-progs`. Certifique-se de que seu sistema está atualizado antes de instalar o pacote.

2. **Adicione o repositório PPA do `Timeshift`**: Antes de instalar o `Timeshift`, é recomendável adicionar o repositório PPA para garantir que você está instalando a versão mais recente. Execute o seguinte comando: `sudo add-apt-repository ppa:teejee2008/timeshift -y`

4. **Atualize a lista de pacotes**: Após adicionar o repositório, atualize a lista de pacotes disponíveis: `sudo apt update`

5. **Instale o `Timeshift`**: Agora, instale o programa com o comando: `sudo apt install timeshift -y`

6. **Configure o `Timeshift`**: Após a instalação, você pode configurar o `Timeshift` executando-o pela primeira vez. Para abrir o `Timeshift` pelo  `Terminal Emulator`, digite: `sudo timeshift-gtk`

    Siga as instruções na tela para configurar as preferências de _backup_, como a escolha entre _backups_ automáticos ou manuais e a definição do tipo de _backup_ (incremental, baseado em `RSYNC` ou `BTRFS`).

Esses comandos devem instalar e configurar o `Timeshift` no seu sistema `Ubuntu`. Caso encontre algum problema durante a instalação, verifique se você possui privilégios de administrador.

O `Timeshift` é inteligente o suficiente para não duplicar arquivos desnecessariamente. Ele utiliza um método chamado `"hard links"` para as cópias de _backup_ quando está operando em sistemas de arquivos que suportam esse recurso, como é o caso do `ext4`.

Aqui está o que acontece quando você marca mais de uma opção de _backup_, como diário, semanal e mensal:

- **Incrementais**: O `Timeshift` cria _backups_ incrementais. Isto significa que após o primeiro _backup_ completo, os _backups_ subsequentes apenas salvam as diferenças em relação ao _backup_ anterior.

- **Hard Links**: Para arquivos que não mudaram entre _snapshots_, o `Timeshift` usa _hard links_. Com _hard links-, vários arquivos em diferentes _snapshots_ podem apontar para o mesmo espaço no disco, economizando espaço. Só é feita uma nova cópia se o arquivo original for alterado.

- **Gerenciamento de _Snapshots_**: Quando você configura diferentes frequências de backup, o `Timeshift` gerencia automaticamente os _snapshots_. Por exemplo, mesmo que um arquivo não tenha mudado desde o último _snapshot_ diário, ele ainda será incluído no _snapshot_ semanal, mas sem consumir espaço extra no disco.

Portanto, mesmo marcando diferentes níveis de _snapshots_ para serem mantidos, o `Timeshift` irá gerenciar inteligentemente o espaço e não criará cópias duplicadas dos arquivos, a menos que haja mudanças neles. Isso otimiza o uso do espaço em disco e mantém os backups organizados e eficientes.

### 1.1 `Type`

Nesta tela do `Timeshift` você pode escolher entre dois tipos de tecnologias de _snapshots_:

- **`RSYNC`**: Este é um método de _backup_ incremental que copia as diferenças entre o estado atual do sistema e o último snapshot. Ele suporta qualquer tipo de sistema de arquivos e é mais flexível em termos de armazenamento, uma vez que os backups podem ser salvos em qualquer local, incluindo unidades externas ou em outra partição. No entanto, como ele copia arquivos, pode ser um pouco mais lento e consumir mais espaço do que o `BTRFS` para as mesmas operações de backup.

- **`BTRFS`**: É um sistema de arquivos com funcionalidades avançadas, incluindo a capacidade de criar _snapshots_ quase instantâneos que não duplicam dados. Seu método de `"copy-on-write"` permite que os backups sejam extremamente rápidos e economizem espaço, uma vez que apenas as alterações desde o último snapshot são armazenadas. Contudo, para usar `BTRFS`, a partição raiz do seu sistema precisa estar usando o sistema de arquivos `BTRFS`, e os _snapshots_ são salvos na mesma partição.

A escolha entre `RSYNC` e `BTRFS` depende do seu sistema de arquivos e de como você deseja gerenciar seus backups. Se você precisa de flexibilidade e está usando diferentes tipos de sistemas de arquivos, ou se deseja armazenar seus backups em locais diferentes, o `RSYNC` é a melhor escolha. Por outro lado, se você já está usando `BTRFS` e deseja a forma mais rápida e eficiente de armazenar _snapshots_ no mesmo dispositivo, a opção `BTRFS` seria ideal.

### 1.2 `Location`

<div align="center">
    <img src="figures/fig1.png" alt="Minha Imagem" />
    <p>Fig. 1.</p>
</div>

Esta tela do `Timeshift` é onde você seleciona o local onde os _snapshots_ (ou seja, os _backups_ do estado do sistema) serão armazenados. Aqui estão os elementos principais que você pode observar na interface:

- **Lista de Discos**: Mostra todos os discos conectados ao seu sistema que têm um sistema de arquivos `Linux` compatível (como `ext4`, que é o mais comum em instalações `Linux`). Você não pode escolher discos com sistemas de arquivos `Windows` (`NTFS` ou `FAT`) e nem locais em rede.

- **Partições**: Cada disco pode ter várias partições, cada uma representada aqui com seu próprio tamanho e espaço livre disponível, bem como rótulos e pontos de montagem (indicados na coluna `"Name"`).

- **Informação Importante**: Na parte inferior da janela, há avisos e restrições importantes sobre a seleção do local do _snapshot_. Por exemplo, os _snapshots_ são salvos na pasta `/timeshift` na partição selecionada, e locais remotos ou de rede não são suportados.

Quando você seleciona uma partição para seus _backups_, é recomendável escolher uma com bastante espaço livre para acomodar vários _snapshots_, especialmente se você planeja manter múltiplas versões de _backups_ ao longo do tempo.

- **Seleção de _Snapshot_**: Você deve escolher uma partição que tenha espaço livre suficiente para armazenar seus _snapshots_. Idealmente, esta partição deve ser diferente daquela onde o sistema operacional está instalado, para que os _backups_ permaneçam seguros caso a partição do sistema seja corrompida ou afetada.


### 1.3 `Schedule`

<div align="center">
    <img src="figures/fig2.png" alt="Minha Imagem" />
    <p>Fig. 2.</p>
</div>

Nesta tela do `Timeshift`, você pode definir a frequência e a quantidade de _snapshots_ que deseja manter no sistema. Vamos detalhar cada parte:

- **`Select _Snapshot_ Levels` (Selecionar Níveis de Snapshot)**: Aqui você pode marcar quais tipos de _snapshots_ quer que o `Timeshift` crie e mantenha automaticamente: mensal, semanal, diário, horário e a cada inicialização do sistema (boot).

    - **`Monthly` (Mensal)**: Determina quantos _snapshots_ mensais você deseja manter.

    - **`Weekly` (Semanal)**: Determina quantos _snapshots_ semanais você deseja manter.

    - **`Daily` (Diário)**: Determina quantos _snapshots_ diários você deseja manter.

    - **`Hourly` (Horário)**: Determina quantos _snapshots_ horários você deseja manter.

        - **`Boot`**: Determina quantos _snapshots_ são criados a cada inicialização do sistema que você deseja manter.

- **`Keep` (Manter)**: Ao lado de cada frequência, há um número com botões de mais e menos que permite aumentar ou diminuir a quantidade de _snapshots_ a manter para cada nível.

- **`Stop cron _e-mails_ for scheduled tasks` (Parar _e-mails_ do `cron` para tarefas agendadas)**: Se marcado, o sistema não enviará _e-mails_ automáticos sempre que uma tarefa `cron` relacionada ao `Timeshift` for executada. Isso pode ser útil para evitar _spam_ no seu _e-mail_ se você tiver o sistema configurado para enviar _e-mails_ de `cron` _jobs_.

- **Informações Adicionais**: Na parte inferior, há informações importantes sobre como os _snapshots_ são agendados e criados:

    - Os _snapshots_ não são agendados em horários fixos, mas sim conforme a necessidade com base na frequência configurada.

    - Uma tarefa de manutenção é executada a cada hora para criar _snapshots_, se necessário.

    - _snapshots_ de boot são criados com um atraso de 10 minutos após a inicialização do sistema.

- **`Scheduled _snapshots_ are enabled` (Snapshots agendados estão ativados)**: Esta mensagem indica que os _snapshots_ automáticos estão configurados e serão criados se houver espaço suficiente no disco (mais de 1 GB).

Lembre-se de que a manutenção de múltiplos _snapshots_ pode ocupar uma quantidade significativa de espaço no disco, então assegure-se de ter espaço suficiente na partição selecionada para armazená-los.

### 1.4 `User`

<div align="center">
    <img src="figures/fig3.png" alt="Minha Imagem" />
    <p>Fig. 3.</p>
</div>

Na tela de configurações do `Timeshift` que você está mostrando, há opções para gerenciar como os diretórios `home` dos usuários são incluídos nos _snapshots_:

- **`Exclude All Files` (Excluir Todos os Arquivos)**: Se selecionada, esta opção exclui todos os arquivos no diretório `home` do usuário do _snapshot_. Isso é útil se você deseja apenas fazer _backup_ do sistema sem os dados pessoais dos usuários.

- **`Include Only Hidden Files` (Incluir Apenas Arquivos Ocultos)**: Essa opção faz com que apenas os arquivos ocultos (aqueles que começam com um ponto .) sejam incluídos no snapshot. Em sistemas `Linux`, arquivos e diretórios ocultos geralmente contêm configurações do usuário e dados de aplicativos, mas não documentos e arquivos de mídia.

- **`Include All Files` (Incluir Todos os Arquivos)**: Ao escolher essa opção, todos os arquivos no diretório `home`, incluindo documentos, imagens, vídeos e outros dados pessoais, além dos arquivos ocultos de configuração, serão incluídos no snapshot. Isso resulta em um _backup_ mais completo, mas também pode consumir significativamente mais espaço em disco.

Portanto, ao selecionar `"Include All Files"`, você está optando por um _backup_ mais abrangente que inclui tanto os dados de configuração quanto os arquivos pessoais dos usuários. Isso pode ser importante para garantir que você não perca nenhum dado em caso de falha do sistema ou quando precisar restaurar um sistema para um estado anterior.

### 1.5 `Filters`

Caso tenha escolhido no Item `User` a opção `Include Only Hidden Files` e/ou `Include All Files`, na tela `"Include / Exclude Patterns"` do `Timeshift`, você pode gerenciar quais arquivos e pastas estão incluídos ou excluídos dos _snapshots_. Vamos entender os elementos da tela:

- **`Patterns` (Padrões)**: A lista mostra os padrões de caminhos de arquivos e diretórios que serão incluídos ou excluídos. Os padrões podem usar curingas como `*` e `**` para representar qualquer número de caracteres ou diretórios.

- **`+/-` Símbolos**: Indicam se um padrão é para inclusão (`+`) ou exclusão (`-`) de arquivos.
Caminhos:

    - **`/nonexistent*/**`** parece ser um padrão de exclusão para uma pasta ou arquivo que não existe (provavelmente um exemplo ou erro).

    - **`/home/edendenis/*`** indica que todos os arquivos e subdiretórios dentro do diretório home do usuário edendenis serão incluídos no snapshot.

    - **`/root/.**`** inclui todos os arquivos ocultos (aqueles que começam com um ponto) no diretório raiz do administrador (`root`).

    - **`/home/edendenis/Documents/***` e `/home/edenedfsfs/Documents/***`** parecem ser padrões de exclusão específicos para os diretórios de documentos de dois usuários.

- **Adicionar e Remover Padrões**:

    - **`Add` (Adicionar)**: Permite adicionar um novo padrão à lista.

    - **`Add Files` (Adicionar Arquivos)**: Permite escolher arquivos específicos para incluir ou excluir.

    - **`Add Folders` (Adicionar Pastas)**: Permite escolher pastas específicas para incluir ou excluir.

        - É recomendado excluir as pastas:

            - `~/.local/share/Trash` de cada usuário

            - `~/Documents` para cada usuário, caso o usuário utilize um serviço de armazenamento em nuvem
            
            - `~/Downloads` de cada usuário, caso o usuário utilize um serviço de armazenamento em nuvem
            
            - `~/Music` para cada usuário, caso o usuário utilize um serviço de armazenamento em nuvem
            
            - `~/Pictures` para cada usuário, caso o usuário utilize um serviço de armazenamento em nuvem
            
            - `~/Videos` para cada usuário, caso o usuário utilize um serviço de armazenamento em nuvem

    - **`Remove` (Remover)**: Remove o padrão selecionado da lista.

    - **`Summary` (Resumo)**: Fornece um resumo das regras de inclusão e exclusão configuradas.
    
Essa funcionalidade é útil para personalizar seus backups, permitindo que você exclua arquivos temporários ou desnecessários e inclua documentos importantes e configurações do sistema. Isso ajuda a economizar espaço em disco e a criar _snapshots_ mais rápidos e eficientes.

### 1.6 `Misc`

Esta tela nas configurações do `Timeshift` permite que você escolha o formato de data e hora usado para nomear os _snapshots_.

- **`Date Format` (Formato da Data)**: No menu suspenso, você pode selecionar diferentes formatos de data e hora. Cada opção mostra um exemplo de como a data e hora serão exibidas, seguida pela string de formato que define esse estilo de exibição.

- **Exemplo de Data e Formato**:

    - O exemplo mostrado parece ser `2019-08-11 20:25:43`, que é um formato comum internacionalmente, representando ano, mês, dia, hora, minuto e segundo.

    - Ao lado do exemplo, você vê a string de formatação correspondente: `%Y-%m-%d %H:%M:%S`. Aqui, `%Y` representa o ano com quatro dígitos, `%m` o mês, `%d` o dia, `%H` a hora no formato 24h, `%M` os minutos, e `%S` os segundos.

A escolha do formato da data e hora é útil para facilitar a identificação dos _snapshots_, especialmente se você precisar encontrar um específico baseado na data e hora em que foi criado. Ao configurar isso, você personaliza como as datas serão mostradas em todos os _snapshots_ listados dentro do Timeshift, de acordo com sua preferência.

### 1.1 Código completo para configurar/instalar/usar

Para configurar/instalar/usar o `timeshift` no `Linux Ubuntu` sem precisar digitar linha por linha, você pode seguir estas etapas:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando: `Ctrl + Alt + T`

2. Digite o seguinte comando e pressione `Enter`:

    ```
    sudo apt clean
    sudo apt autoclean
    sudo apt autoremove -y
    sudo apt update
    sudo apt --fix-broken install
    sudo apt clean
    sudo apt list --upgradable
    sudo apt full-upgrade -y
    sudo add-apt-repository -y ppa:teejee2008/timeshift
    sudo apt install timeshift -y
    sudo apt update -y
    sudo timeshift-gtk
    ```


## 2. Desinstalar o `Timeshift` pelo `Terminal Emulator`

Para desinstalar o `Timeshift` pelo `Terminal Emulator` no `Linux Ubuntu`, você pode seguir estes passos:

1. **Abra o `Terminal Emulator`**: Você pode fazer isso pressionando `Ctrl+Alt+T` ou procurando por `"Terminal"` no menu de aplicações.

2. **Desinstalar o `Timeshift`**: Execute o seguinte comando para remover o `Timeshift` do seu sistema: `sudo apt remove timeshift -y`

3. **Limpar pacotes não necessários**: Após remover o `Timeshift`, pode ser uma boa ideia limpar os pacotes que não são mais necessários, o que pode ajudar a liberar espaço no disco. Execute o comando: `sudo apt autoremove -y`

4. **Remover configurações residuais**: Se você deseja também remover as configurações e dados locais que foram usados pelo `Timeshift`, você pode executar:

    ```
    sudo rm -rf /etc/timeshift
    sudo rm -rf /var/log/timeshift
    ```

Esses comandos removerão o `Timeshift` e suas configurações, liberando espaço e removendo as configurações que foram personalizadas para o uso no seu sistema.


## 3.  Restaurar o Sistema com `Timeshift` no LiveUSB

### 3.1 **Preparação: Montar Partições**

1 **Monte a partição raiz `(/`)**:

```
sudo mount /dev/sda2 /mnt
```

2 **Monte a partição `/boot`**:

```
sudo mount /dev/sda6 /mnt/boot
```

3 **Monte os diretórios necessários para funcionamento do sistema**:

```
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
```

### 3.2 Instale e Configure o Timeshift

1. **Monte a partição onde os backups estão armazenados**:

```
sudo mount /dev/sda5 /mnt/timeshift
```

2. **Instale o `Timeshift` (se necessário)**:

```
sudo apt update
sudo apt install timeshift -y
```

3. **Verifique o UUID da partição do backup e edite a configuração (opcional)**:

1 **Identifique o UUID**:

```
blkid
```

2 **Edite o arquivo de configuração**:

```
sudo nano /etc/timeshift/timeshift.json
```

3 Atualize o campo `"backup_device_uuid"` com o UUID correto da partição do _backup_.

### 3.3 Restaurar o Sistema com Timeshift

1. **Execute o `Timeshift` em modo texto**:

```
sudo timeshift --restore
```

2. **Escolha o snapshot desejado**: O programa listará os snapshots. Digite o número correspondente ao snapshot que deseja restaurar.

3. **Responda às perguntas do processo**:

3.1 **Reinstalar o GRUB? Digite**:

```
y
```

3.2 **Selecione o disco para o GRUB**: Escolha o disco principal (geralmente `/dev/sda`).

4. **Confirme a restauração**: Quando solicitado, confirme com:

```
y
```

### 3.4 Finalizar a Restauração

1. **Saia do Ambiente de Restauração**: Se você estiver no ambiente chroot, saia:

```
exit
```

2. **Desmonte as Partições Montadas**:

```
sudo umount /mnt/dev
sudo umount /mnt/proc
sudo umount /mnt/sys
sudo umount /mnt/boot
sudo umount /mnt
```

3. **Reinicie o Computador**: Remova o LiveUSB antes de reiniciar para que o sistema inicialize do disco restaurado:

```
sudo reboot
```

## Referências

[1] OPENAI. ***Instalar `Timeshift` via terminal.*** Disponível em: <https://chat.openai.com/c/4b997bc7-af50-402f-9211-99d29fdd10a8> (texto adaptado). Acessado em: 23/04/2023 17:11.

[2] OPENAI. ***Vs code: editor popular.*** Disponível em: <https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42> (texto adaptado). Acessado em: 23/04/2024 17:10.

