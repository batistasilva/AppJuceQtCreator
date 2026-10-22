# AppJuceQtCreator
Just an Application to show how to use QtCreator with JUCE Framework...

# 🚀 Explorando o Framework JUCE: Do Desktop ao Android

Bem-vindo ao guia prático sobre o **JUCE**, o framework em C++ mais utilizado no mundo para o desenvolvimento de aplicações e plugins de áudio de alta performance. 

Este repositório serve como um ponto de partida para iniciantes, cobrindo desde o conceito básico até a estruturação de um projeto utilizando boas práticas modernas de desenvolvimento.

---

## 🌟 O que é o Framework JUCE?

O **JUCE (Guided Hand-Shed Software)** é um framework cross-platform especializado em áudio digital e interfaces gráficas (GUI). Com ele, você escreve o código uma única vez em C++ e o exporta para Windows, macOS, Linux, iOS e Android.

### O que você pode construir com ele?
* 🎛️ **Plugins de Áudio:** Crie seus próprios efeitos (Delay, Reverb, Distorção) ou Instrumentos Virtuais (Sintetizadores) nos formatos mais populares do mercado: VST3, AU, AAX e CLAP.
* 📈 **Aplicações Gráficas Avançadas:** Crie softwares standalone de áudio (como DAWs simplificadas ou reprodutores de mídia) com renderização gráfica 2D/3D acelerada por hardware.
* 🎮 **Áudio para Jogos:** Desenvolva sistemas de áudio interativos e dinâmicos para engines ou middlewares de jogos.
* 📱 **Aplicações Mobile:** Leve suas ferramentas de produção musical e sintetizadores direto para a tela de smartphones e tablets.

---

## 1. Como Instalar o JUCE (Passo a Passo)

### 🐧 No Linux (Ubuntu/Debian)
Para compilar projetos JUCE no Linux, precisamos instalar as dependências de áudio (ALSA, JACK) e desenvolvimento gráfico.

1. **Atualize o sistema e instale as dependências essenciais:**
   ```bash
   sudo apt update
   sudo apt install build-essential cmake git libasound2-dev libjack-jackd2-dev                     ladspa-sdk-dev libcurl4-openssl-dev libfreetype6-dev                     libx11-dev libxext-dev libxinerama-dev libxrandr-dev                     libxcursor-dev libwebkit2gtk-4.0-dev libglu1-mesa-dev
   ```
2. **Clone o repositório oficial do JUCE:**
   ```bash
   git clone --branch 9.0.4 https://github.com/juce-framework/JUCE.git 

   ```

### 🪟 No Windows
1. **Instale o Visual Studio:** Baixe o [Visual Studio Community](https://visualstudio.microsoft.com/) e, durante a instalação, selecione a carga de trabalho **"Desenvolvimento para desktop com C++"**.
2. **Instale o CMake:** Baixe e instale o [CMake oficial](https://cmake.org/download/), certificando-se de adicioná-lo ao PATH do sistema durante a instalação.
3. **Instale o Git:** Instale o [Git para Windows](https://git-scm.com/).
4. **Clone o JUCE:** Abra o Prompt de Comando ou PowerShell e execute:
   ```cmd
   git clone --branch master https://github.com/juce-framework/JUCE.git C:\JUCE
   ```

---

## 2. Criando sua Primeira Aplicação no Qt Creator

Para quem já tem instalado basta dar sequencia no processo de criação do scrip do cmake e testar
a aplicação de exemplo.

Para quem ainda não tem instalado e usa linux pode instalar a versão da comunidade atraves dos pacotes de instalação do sistema, ou pode baixar a ultima versão do site official do qt.

Baixando o Qt6, para Linux, Windows ou MacOS: https://download.qt.io/official_releases/qtcreator/latest/

Se preferir compile sua propria versão baixando o código do github para versão v20.0.2: https://github.com/qt-creator/qt-creator/tree/v20.0.2

As instruções de compilação estão na página.

Embora o JUCE possua uma ferramenta própria chamada *Projucer*, a indústria utiliza predominantemente o **CMake** para gerenciar projetos modernos de médio e grande porte. O **Qt Creator** oferece um suporte nativo fantástico para CMake.


Duvidas: Peça ajuda para IA:(DeepSeek, Craude, ChatGPT ou Gemini)

> *⚠️ **Atenção:** Apesar de estarmos usando a IDE Qt Creator, não usaremos as bibliotecas do Qt. Usaremos apenas a interface gráfica e o compilador da IDE para desenvolver em JUCE nativo.*

### Estrutura de Arquivos do Projeto
Crie uma pasta para o seu projeto com a seguinte estrutura básica:
```text
MeuProjetoJUCE/
├── CMakeLists.txt
└── Main.cpp
```

### 📄 Código do `CMakeLists.txt` para compilar uma aplicação feita em JUCE

Substitua o conteúdo do arquivo de configuração pelo script moderno abaixo. *(Ajuste o caminho do JUCE conforme sua instalação).*

```cmake
cmake_minimum_required(VERSION 3.15)
project(MinhaAppJUCE VERSION 1.0.0 LANGUAGES CXX)

# Define a versão do C++ requerida pelo JUCE
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Altere o caminho abaixo para onde você clonou o JUCE na sua máquina
if(CMAKE_HOST_SYSTEM_NAME STREQUAL "Windows")
    add_subdirectory("C:/JUCE" juce_cmake_helper)
else()
    add_subdirectory("$ENV{HOME}/JUCE" juce_cmake_helper)
endif()

# Cria uma aplicação com interface gráfica (GUI)
juce_add_gui_app(MinhaAppJUCE)

# Adiciona o arquivo de código fonte
target_sources(MinhaAppJUCE PRIVATE Main.cpp)

# Organiza os módulos e vincula as dependências fundamentais de áudio e interface
target_link_libraries(MinhaAppJUCE PRIVATE
    juce::juce_audio_basics
    juce::juce_graphics
    juce::juce_gui_basics
)

# Habilita formatos de arquivo comuns e recursos básicos
target_compile_definitions(MinhaAppJUCE PRIVATE
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
)
```

### 📄 Código de Teste do `Main.cpp`
Este é um template limpo seguindo as diretrizes modernas de arquitetura do JUCE. Ele cria uma janela nativa centralizada que exibe uma mensagem na tela.

```cpp
#include <JuceHeader.h>

//==============================================================================
class MainComponent  : public juce::Component
{
public:
    MainComponent()
    {
        setSize (400, 300);
    }

    ~MainComponent() override = default;

    void paint (juce::Graphics& g) override
    {
        // Define o fundo com uma cor escura elegante (Boas práticas de UI de áudio)
        g.fillAll (juce::Colours::black);

        // Configura a fonte e renderiza o texto centralizado
        g.setFont (juce::FontOptions (24.0f, juce::FontOptions::FontStyleFlags::bold));
        g.setColour (juce::Colours::cyan);
        g.drawText ("Olá, JUCE + Qt Creator!", getLocalBounds(), juce::Justification::centred, true);
    }

    void resized() override {}

private:
    JUCE_DECLARE_NON_COPYABLE_WITH_LEAK_DETECTOR (MainComponent)
};

//==============================================================================
class MainWindow    : public juce::DocumentWindow
{
public:
    MainWindow (juce::String name)
        : DocumentWindow (name,
                          juce::Desktop::getInstance().getDefaultLookAndFeel()
                                                      .findColour (juce::ResizableWindow::backgroundColourId),
                          DocumentWindow::allButtons)
    {
        setUsingNativeTitleBar (true);
        setContentOwned (new MainComponent(), true);

       #if JUCE_IOS || JUCE_ANDROID
        setFullScreen (true);
       #else
        setResizable (true, true);
        centreWithSize (getWidth(), getHeight());
       #endif

        setVisible (true);
    }

    void closeButtonPressed() override
    {
        juce::JUCEApplication::getInstance()->systemRequestedQuit();
    }

private:
    JUCE_DECLARE_NON_COPYABLE_WITH_LEAK_DETECTOR (MainWindow)
};

//==============================================================================
class MinhaAppJUCEApplication  : public juce::JUCEApplication
{
public:
    MinhaAppJUCEApplication() = default;

    const juce::String getApplicationName() override       { return "MinhaAppJUCE"; }
    const juce::String getApplicationVersion() override    { return "1.0.0"; }
    bool moreThanOneInstanceAllowed() override             { return true; }

    void initialise (const juce::String& commandLine) override
    {
        mainWindow.reset (new MainWindow (getApplicationName()));
    }

    void shutdown() override
    {
        mainWindow.reset();
    }

    void systemRequestedQuit() override
    {
        quit();
    }

    void anotherInstanceStarted (const juce::String& commandLine) override {}

private:
    std::unique_ptr<MainWindow> mainWindow;
};

//==============================================================================
START_JUCE_APPLICATION (MinhaAppJUCEApplication)
```

#### Como rodar no Qt Creator:
1. Abra o **Qt Creator**.
2. Vá em *File > Open File or Project* e selecione o arquivo `CMakeLists.txt`.
3. Escolha o seu Kit de compilação desktop padrão e clique em **Configure Project**.
4. Pressione `Ctrl + R` (ou clique no botão verde "Run") para compilar e executar!

---

## 3. Requisitos para Exportar para Android

O JUCE foi desenhado para facilitar a portabilidade mobile. No entanto, compilar código nativo C++ para rodar dentro da máquina virtual do Android exige algumas ferramentas específicas do Google.

### 📋 O que você precisa instalar:
1. **Java Development Kit (JDK):** Instale o JDK 11 ou 17 (versões mais estáveis e suportadas pelas ferramentas do Android Studio).
2. **Android Studio:** A IDE oficial do Google, necessária principalmente para baixar e gerenciar as dependências do ecossistema.
3. **Android SDK & NDK (Native Development Kit):**
   * Dentro do Android Studio, abra o *SDK Manager*.
   * Baixe a versão mais recente da API do Android (SDK).
   * Vá na aba **SDK Tools** e marque a opção **NDK (Side by side)** e **CMake**. *O NDK é o componente mais importante, pois é ele quem compila seu código C++ diretamente para a arquitetura de processadores mobile (ARM/ARM64).*

### 🛠️ Integrando e Compilando
* **Abordagem com o Projucer:** Se você preferir usar a interface visual do JUCE, você pode adicionar um target de exportação chamado **Android**. Nele, você cola o caminho exato onde salvou o SDK e o NDK. Ao clicar em salvar, o JUCE gerará uma pasta inteira com uma estrutura do Android Studio (comGradle), pronta para você abrir, emular e gerar seu `.apk`.
* **Abordagem com CMake:** Você pode usar o arquivo de *toolchain* oficial fornecido dentro da pasta do Android NDK para fazer a compilação cruzada diretamente do terminal ou mapeando o compilador do Android NDK dentro dos Kits do Qt Creator.

### 🎧 Boas Práticas para Áudio Mobile
* **Mecanismo de Baixa Latência (Oboe):** Dispositivos Android possuem fragmentação extrema de hardware. Internamente, o JUCE já integra o **Oboe**, uma biblioteca de alta performance desenvolvida pelo Google que decide dinamicamente se usará a API *AAudio* (moderna e rápida) ou *OpenSL ES* (legada). Garanta que seus buffers de áudio sejam múltiplos pequenos para tirar vantagem disso.
* **Gerenciamento de Permissões:** Se seu app for processar áudio de entrada (como um microfone ou interface de som externa), não se esqueça de adicionar explicitamente a permissão de gravação de áudio no manifesto do Android (`RECORD_AUDIO`).

