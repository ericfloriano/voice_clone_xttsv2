# Exemplo de Código para Clonagem de Voz com IA no Google Colab

Este documento apresenta um exemplo de código Python para realizar a clonagem de voz utilizando inteligência artificial no ambiente Google Colab. O modelo utilizado para esta tarefa é o `xtts`.

**Recursos Adicionais:**

* **Modelo Utilizado:** `xtts`
* **Documentação do Coqui AI (TTS):** [https://docs.coqui.ai/en/latest/](https://docs.coqui.ai/en/latest/)

---

## Código Completo

```python
import locale
locale.getpreferredencoding = lambda: "UTF-8"

# Opcional: Download de arquivo de áudio de referência
# !wget "<file/path file/URL>" -O <fileName.extension>

# Opcional: Conversão de arquivo de áudio de referência
# !ffmpeg -i <fileName.extension> <convertedFileName.extension>

!pip install TTS

import torch
from TTS.api import TTS
import time
from IPython.display import Audio, display

# Carregando o modelo xtts_v2 (certifique-se de ter uma GPU disponível para melhor desempenho)
tts = TTS("tts_models/multilingual/multi-dataset/xtts_v2").to('cuda')

# Texto a ser sintetizado
texto = """
bla bla bla
"""

# Gerando o áudio com a função tts_to_file()
start = time.time()
res = tts.tts_to_file(text=texto, speaker_wav="<caminho/para/seu/arquivo/de/referencia.wav>", language= 'pt', emotion="Happy", speed=2.5, file_path="out.wav")
print(res)
tempo = time.time() - start
print(f"Tempo total: {tempo}")

# Reproduzindo o áudio gerado
display(Audio('out.wav', autoplay=True))

```
## Explicação Detalhada do Código

**`import locale`**:
* **Objetivo:** Importa o módulo `locale` do Python. Este módulo fornece acesso a funcionalidades de localização e internacionalização, permitindo que o programa se adapte a diferentes configurações regionais e idiomas. No contexto deste código, é utilizado para garantir o correto tratamento da codificação de caracteres.

**`locale.getpreferredencoding = lambda: "UTF-8"`**:
* **Objetivo:** Esta linha redefine a função `locale.getpreferredencoding` para sempre retornar a string `"UTF-8"`. UTF-8 é uma codificação de caracteres amplamente utilizada que suporta uma vasta gama de caracteres de diferentes idiomas. Ao forçar o uso de UTF-8, evita-se problemas de compatibilidade ao lidar com textos em diversas línguas.

**`# !wget "<file/path file/URL>" -O <fileName.extension>`**:
* **Objetivo:** Esta linha (comentada com `#`) utiliza o comando `wget` para baixar um arquivo da web diretamente para o ambiente do Google Colab.
    * **`!wget`:** O prefixo `!` permite executar comandos de shell dentro de uma célula do Colab.
    * **`"<file/path file/URL>"`:** Deve ser substituído pela URL do arquivo que você deseja baixar ou pelo caminho do arquivo no seu Google Drive (se estiver acessando um arquivo local).
    * **`-O <fileName.extension>`:** A opção `-O` especifica o nome e a extensão com os quais o arquivo baixado será salvo.
* **Observação:** Esta etapa é opcional e geralmente é utilizada para baixar o arquivo de áudio de referência que será usado para clonar a voz.

**`# !ffmpeg -i <fileName.extension> <convertedFileName.extension>`**:
* **Objetivo:** Esta linha (comentada com `#`) utiliza o comando `ffmpeg` para converter um arquivo de áudio ou vídeo de um formato para outro. `ffmpeg` é uma ferramenta poderosa para manipulação de multimídia.
    * **`!ffmpeg`:** Permite executar comandos de shell.
    * **`-i <fileName.extension>`:** Especifica o arquivo de entrada (o arquivo que você deseja converter).
    * **`<convertedFileName.extension>`:** Especifica o nome e a extensão do arquivo de saída (o arquivo convertido).
* **Observação:** Esta etapa também é opcional, mas pode ser necessária para garantir que o arquivo de áudio de referência esteja em um formato compatível com o modelo `xtts`.

**`!pip install TTS`**:
* **Objetivo:** Utiliza o gerenciador de pacotes `pip` para instalar a biblioteca `TTS` (Text-to-Speech). A biblioteca `TTS` é desenvolvida pelo Coqui AI e fornece as ferramentas necessárias para realizar a síntese de voz e a clonagem de voz.
* **`!pip install TTS`:** Garante que a biblioteca `TTS` e suas dependências sejam instaladas no ambiente do Colab.

**`import torch, from TTS.api import TTS, import time, from IPython.display import Audio, display`**:
* **Objetivo:** Importam os módulos Python necessários para o código:
    * **`import torch`:** Importa a biblioteca `torch`, essencial para computação numérica e redes neurais em PyTorch, que é a base da biblioteca `TTS`.
    * **`from TTS.api import TTS`:** Importa a classe `TTS` da biblioteca `TTS`, que fornece a interface principal para interagir com os modelos de text-to-speech.
    * **`import time`:** Importa o módulo `time` para medir o tempo de execução de diferentes partes do código.
    * **`from IPython.display import Audio, display`:** Importa as funções `Audio` e `display` do módulo `IPython.display`, utilizadas para exibir e reproduzir áudio diretamente na saída do Colab.

**`tts = TTS("tts_models/multilingual/multi-dataset/xtts_v2").to('cuda')`**:
* **Objetivo:** Inicializa o modelo de text-to-speech `xtts_v2`.
    * **`TTS("tts_models/multilingual/multi-dataset/xtts_v2")`:** Cria uma instância da classe `TTS` e carrega o modelo `xtts_v2`. Este é um modelo multilíngue capaz de clonar vozes a partir de um áudio de referência.
    * **`.to('cuda')`:** Move o modelo para a GPU (`cuda`) se uma estiver disponível no ambiente do Colab. A execução na GPU acelera significativamente o processo de síntese de voz. Se a GPU não estiver disponível, o modelo será executado na CPU.

**`texto = """ bla bla bla """`**:
* **Objetivo:** Define a variável `texto` que contém o texto a ser convertido em áudio com a voz clonada.
* **`"""bla bla bla"""`:** Uma string de múltiplas linhas que contém o texto desejado. Substitua `"bla bla bla"` pelo texto que você quer que seja sintetizado.

**`start = time.time(), res = tts.tts_to_file(...), print(res), tempo = time.time() - start, print(f"Tempo total: {tempo}")`**:
* **Objetivo:** Geram o áudio com a função `tts_to_file()` e medem o tempo de execução.
    * **`start = time.time()`:** Marca o início da execução da função `tts_to_file()`.
    * **`res = tts.tts_to_file(text=texto, speaker_wav="<caminho/para/seu/arquivo/de/referencia.wav>", language= 'pt', emotion="Happy", speed=2.5, file_path="out.wav")`**: Chama a função `tts_to_file()` para gerar o áudio.
        * **`text=texto`:** O texto a ser sintetizado.
        * **`speaker_wav="<caminho/para/seu/arquivo/de/referencia.wav>"`:** O caminho para o arquivo de áudio de referência para clonar a voz. **Substitua pelo caminho correto.**
        * **`language='pt'`:** Define o idioma como português.
        * **`emotion="Happy"`:** Define a emoção da voz.
        * **`speed=2.5`:** Define a velocidade da fala.
        * **`file_path="out.wav"`:** Define o nome do arquivo de saída.
    * **`print(res)`:** Imprime o resultado da síntese.
    * **`tempo = time.time() - start`:** Calcula o tempo total da síntese.
    * **`print(f"Tempo total: {tempo}")`:** Imprime o tempo total de execução.

**`display(Audio('out.wav', autoplay=True))`**:
* **Objetivo:** Reproduz o áudio gerado diretamente no Colab.
* **`Audio('out.wav', autoplay=True)`:** Cria um objeto `Audio` com o arquivo gerado. `autoplay=True` inicia a reprodução automaticamente.
* **`display(...)`:** Exibe o player de áudio.

## Observações Importantes

* **Substitua os Placeholders:** Certifique-se de substituir `<file/path file/URL>`, `<fileName.extension>`, e `<caminho/para/seu/arquivo/de/referencia.wav>` pelos seus valores corretos.
* **Arquivo de Referência:** O arquivo em `speaker_wav` é crucial para a clonagem da voz. Use um arquivo de áudio de boa qualidade.
* **GPU:** Usar uma GPU acelera significativamente o processo.
* **Personalização:** Ajuste os parâmetros de `tts_to_file()` para modificar a saída.
* **Documentação:** Consulte a documentação do Coqui AI para mais detalhes.
* **Agradecimento:** Código baseado no conhecimento de SauloCatharino.
