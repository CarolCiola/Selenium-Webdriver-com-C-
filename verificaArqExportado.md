# Verificador de exportação de arquivo

Criaremos a seguir um método que contém a função que verifica se um determinado arquivo foi baixado após uma operação no sistema, como clicar no botão "Download", por exemplo.<p>
<b>Atenção:</b> utilizaremos aqui o conceito de <a href="https://pt.wikipedia.org/wiki/Programa%C3%A7%C3%A3o_orientada_a_objetos"><b>POO</b></a>, portanto teremos a classe <i><b>principal.cs</b></i> e a classe do nosso método, <i><b>exportacao.cs</b></i>.
<br>

# exportacao.cs

```csharp
        public bool VerificaArquivoBaixado(string nomeArquivo)
        {
            bool existe = false;
	          
            string pathUser = Environment.GetFolderPath(Environment.SpecialFolder.UserProfile);
            string pathDownload = Path.Combine(pathUser, @"Downloads\");

            File.Delete(pathDownload + nomeArquivo + ".pdf");

            IWebElement lnkImprimir = driver.FindElement(By.Id("idBotao"));
            lnkImprimir.Click();
            Thread.Sleep(1000);

            string[] filePaths = Directory.GetFiles(pathDownload);
            foreach (string p in filePaths)
            {
                if (p.Contains(nomeArquivo + ".pdf"))
                {
                    existe = true;
                    //Deleta o arquivo gerado
                    File.Delete(pathDownload + nomeArquivo + ".pdf");
                    break;                    
                }
            }

            if (existe == false)
            {
                driver.Quit();
            }

            return existe;
        }
```
A classe é pública e booleana, ou seja, retornará uma condição <b>true</b> ou <b>false</b>.
A string <b>nomeArquivo</b> recebe da classe principal o nome do arquivo que será gerado, sem a extensão do arquivo.
Iniciamos a variável <b>existe</b> com a condição <b>false</b>, tomando como exemplo de que o arquivo ainda não existe.

```csharp
        public bool VerificaArquivoBaixado(string nomeArquivo)
        {
            bool existe = false;
```
O trecho abaixo obtém o caminho da pasta "Downloads" do perfil do usuário da máquina, e atribui à variável <b>pathDownload</b>:
```csharp            

            string pathUser = Environment.GetFolderPath(Environment.SpecialFolder.UserProfile);
            string pathDownload = Path.Combine(pathUser, @"Downloads\");
```            
Deletamos os arquivos já existentes na pasta que possuem o mesmo nome do arquivo que iremos gerar, para garantir que nosso script leia o arquivo correto gerado durante a automação do teste.
```csharp     
            File.Delete(pathDownload + nomeArquivo + ".pdf");
```     
Este é o momento em que clicamos no botão de download no sistema:
```csharp  
            IWebElement btnDownload = driver.FindElement(By.Id("idBotao"));
            btnDownload.Click();
```
Abaixo verificamos se o arquivo foi salvo na pasta Downloads, após o clique no botão.
```csharp  
            string[] filePaths = Directory.GetFiles(pathDownload);
            foreach (string p in filePaths)
            {
                if (p.Contains(nomeArquivo + ".pdf"))
                {
                    existe = true;

                    File.Delete(pathDownload + nomeArquivo + ".pdf");
                    break;                    
                }
            }

            if (existe == false)
            {
                driver.Quit();
            }

            return existe;
        }
```
- <b>filePaths</b> recebe o caminho do diretório onde está a pasta <b>Downloads</b>.
- Tendo em mente que é inviável limpar todo o conteúdo da pasta <b>Downloads</b> sempre que formos executar nosso script, o <b>foreach</b> efetua a seguinte verificação: 
para cada um dos arquivos existentes dentro da pasta Downloads (representados pela variável "<b>p</b>"), verifique se o arquivo possui o <b>nomeArquivo</b> que queremos + a extensão.
- Caso o arquivo seja encontrado, o <b>if</b> atribui à variável <b>existe</b> o valor <b>true</b>, e depois disso, exclui o arquivo. O método retorna, então, o valor de <b>existe</b> = <b>true</b> ao script principal.
- Caso o arquivo não seja encontrado, um outro <b>if</b> verifica se a condição de <ib>existe</b> continua sendo <b>false</b>. Em caso positivo, fecha o navegador, pois o teste falhou. 

# principal.cs

Nesta classe incluímos o seguinte trecho para leitura de nosso método:
```csharp  
    string nomeArquivo = "NomeDoArquivoExportado"; //nome do arquivo sem extensão
    exporta.VerificaArquivoBaixado(nomeArquivo);
```  
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
