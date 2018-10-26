# Verificador de exporta��o de arquivo

Criaremos a seguir um m�todo que cont�m a fun��o que verifica se um determinado arquivo foi baixado ap�s uma opera��o no sistema, como clicar no bot�o "Download", por exemplo.
<b>Aten��o:</b> utilizaremos aqui o conceito de <a href="https://pt.wikipedia.org/wiki/Programa%C3%A7%C3%A3o_orientada_a_objetos"><b>POO</b></a>, portanto teremos a classe <i><b>principal.cs</b></i> e a classe do nosso m�todo, <i><b>exportacao.cs</b></i>.
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
A classe � p�blica e booleana, ou seja, retornar� uma condi��o <i>true</i> ou <i>false</i>.
A string <i>nomeArquivo</i> recebe da classe principal o nome do arquivo que ser� gerado, sem a extens�o do arquivo.
Iniciamos a vari�vel <i>existe</i> com a condi��o <i>false</i>, tomando como exemplo de que o arquivo ainda n�o existe.

```csharp
        public bool VerificaArquivoBaixado(string nomeArquivo)
        {
            bool existe = false;
```
O trecho abaixo obt�m o caminho da pasta "Downloads" do perfil do usu�rio da m�quina, e atribui � vari�vel <i>pathDownload</i>:
```csharp            

            string pathUser = Environment.GetFolderPath(Environment.SpecialFolder.UserProfile);
            string pathDownload = Path.Combine(pathUser, @"Downloads\");
```            
Deletamos os arquivos j� existentes na pasta que possuem o mesmo nome do arquivo que iremos gerar, para garantir que nosso script leia o arquivo correto gerado durante a automa��o do teste.
```csharp     
            File.Delete(pathDownload + nomeArquivo + ".pdf");
```     
Este � o momento em que clicamos no bot�o de download no sistema:
```csharp  
            //Imprime consulta
            IWebElement btnDownload = driver.FindElement(By.Id("idBotao"));
            lnkImprimir.Click();
```
Abaixo verificamos se o arquivo foi salvo na pasta Downloads, ap�s o clique no bot�o.
```csharp  
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
- <i>filePaths</i> recebe o caminho do diret�rio onde est� a pasta <i>Downloads</i>.
- Tendo em mente que � invi�vel limpar todo o conte�do da pasta <i>Downloads</i> sempre que formos executar nosso script, o <i>foreach</i> efetua a seguinte verifica��o: 
para cada um dos arquivos existentes dentro da pasta Downloads (representados pela vari�vel "<i>p</i>"), verifique se o arquivo possui o <i>nomeArquivo</i> que queremos + a extens�o.
- Caso o arquivo seja encontrado, o <i>if</i> atribui � vari�vel <i>existe</i> o valor <i>true</i>, e depois disso, exclui o arquivo. O m�todo retorna, ent�o, o valor de <i>existe</i> = <i>true</i> ao script principal.
- Caso o arquivo n�o seja encontrado, um outro <i>if</i> verifica se a condi��o de <i>existe</i> continua sendo <i>false</i>. Em caso positivo, fecha o navegador, pois o teste falhou. 

# principal.cs

Nesta classe inclu�mos o seguinte trecho para leitura de nosso m�todo:
```csharp  
    string nomeArquivo = "NomeDoArquivoExportado"; //nome do arquivo sem extens�o
    exporta.VerificaArquivoBaixado(nomeArquivo);
```  
<br></br>
D�vidas me contate! carol.ciola@gmail.com
