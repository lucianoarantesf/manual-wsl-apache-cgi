# Guia de Instalação e Configuração do Ubuntu via WSL e Pacotes Apache e CGI

Este manual descreve de forma simples e acadêmica como configurar o Ubuntu no Windows via WSL, instalar e configurar o servidor Apache e o módulo CGI. Siga os passos cuidadosamente para evitar problemas durante a configuração.

---

## 1. Instalação do WSL com Ubuntu

### Passo 1: Abrir o PowerShell
Pressione `Win + X` e escolha **Windows PowerShell**.

### Passo 2: Instalar o Ubuntu
Digite o comando:
```bash
wsl --install -d Ubuntu
```

- O Ubuntu será baixado e instalado.
- Ao final, o sistema pedirá para reiniciar o computador.

### Passo 3: Continuar a instalação
Após reiniciar, o sistema continuará a instalação automaticamente. Caso ocorra algum erro:
1. Pesquise por "Ubuntu" no menu Iniciar.
2. Execute como administrador.
3. O processo continuará. Ao final, será solicitado que você crie um nome de usuário e senha.

### Passo 4: Atualizar pacotes do sistema
Execute o seguinte comando:
```bash
sudo apt-get update -y && sudo apt-get upgrade -y
```

---

## 2. Acesso às Pastas do WSL

### Passo 5: Localizar a pasta do Linux no Windows
1. Abra o Explorador de Arquivos.
2. Digite o caminho abaixo na barra de endereços:
```bash
\\wsl$\Ubuntu
```

3. Para facilitar o acesso, volte uma pasta, crie um atalho para a pasta **Ubuntu** e coloque-o na área de trabalho.

---

## 3. Instalação do Servidor Apache

### Passo 6: Instalar pacotes necessários
Execute o comando:
```bash
sudo apt-get install joe wget p7zip-full curl build-essential zlib1g-dev libcurl4-gnutls-dev
```

### Passo 7: Limpeza do sistema
Remova pacotes desnecessários:
```bash
sudo apt-get autoremove
sudo apt-get autoclean
```

### Passo 8: Instalar o Apache
Verifique se o Apache não está previamente instalado para evitar conflitos na porta 80. Caso não esteja, instale com:
```bash
sudo apt-get install apache2
```

---

## 4. Configuração do Módulo CGI

### Passo 9: Habilitar módulos necessários
```bash
sudo a2enmod cgi
sudo a2enmod rewrite
```

### Passo 10: Gerenciar o Apache
- Pare o Apache:
```bash
sudo service apache2 stop
```

- Inicie o Apache:
```bash
sudo service apache2 start
```

- Verifique o status do Apache:
```bash
sudo service apache2 status
```

---

## 5. Configuração do CGI

### Passo 11: Permissões na pasta CGI
Conceda permissões à pasta, caso necessário:
```bash
sudo chmod 777 /usr/lib/cgi-bin/
```

### Passo 12: Adicionar API na pasta CGI
1. Coloque os arquivos da API na pasta:
```bash
\\wsl$\Ubuntu\usr\lib\cgi-bin
```

2. Acesse a pasta via terminal:
```bash
cd /usr/lib/cgi-bin
```

3. Liste os arquivos na pasta:
```bash
ls
```

4. Conceda permissões aos arquivos na pasta:
```bash
sudo chmod 775 {nomedoarquivo}
```
Exemplo:
```bash
sudo chmod 775 apiportal
```

### Passo 13: Resolver problemas de autenticação com `.htaccess`
Se houver problemas de loop na autenticação básica, certifique-se de colocar o arquivo `.htaccess` na pasta **cgi-bin**.

### Passo 14: Configuração do Apache para suportar `.htaccess`
1. Edite o arquivo de configuração do Apache:
```bash
sudo nano /etc/apache2/apache2.conf
```

2. Altere a linha `AllowOverride none` para:
```bash
AllowOverride All
```

3. Pare e reinicie o Apache:
```bash
sudo service apache2 stop
sudo service apache2 start
```

4. Configure o arquivo CGI:
```bash
sudo nano /etc/apache2/conf-enabled/serve-cgi-bin.conf
```

5. Altere `AllowOverride none` para:
```bash
AllowOverride All
```

6. Reinicie o Apache novamente:
```bash
sudo service apache2 stop
sudo service apache2 start
```

---

## 6. Instalar Pacotes Adicionais para CGI
Para utilizar `libcairo` no CGI, execute:
```bash
sudo apt-get install -y libcairo2
sudo apt-get install -y libpangocairo-1.0-0
sudo apt-get install -y libpango-1.0-0
```

---

Seguindo esses passos, você terá um ambiente Ubuntu via WSL configurado com Apache e suporte a CGI. Certifique-se de testar os serviços e ajustar as permissões conforme necessário.
