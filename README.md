![GitHub License](https://img.shields.io/github/license/lbruss/lab-redes02)

# # Laboratório de IoT 01 – Arduino Web Server + Controle de LED (ON/OFF) – Documentação (Dias 1, 2 e 3 - 11,13,16/03/2026)**

> Documentação extensa

**Integrantes do grupo:**

*   Bruss Loza
*   Daniel
*   Ezequiel

**Professor:**

* José de Assis

***

## 1. Objetivo

Transformar um **Arduino UNO** com **Ethernet Shield W5100** em um **Arduino Web Server** e, ao final, controlar um **LED vermelho real** pela página web (botões **ON** e **OFF**), funcionando:

1.  No computador (via cabo de rede)
2.  No celular (via Wi‑Fi do roteador) — **o foco principal**

**Resumo do que foi feito (3 dias):**

1.  Montagem e validação de rede (roteador, cabos, DHCP, reserva de IP, ping)
2.  Criação de página HTML/CSS e hospedagem no Arduino (Web Server)
3.  Controle físico (LED no pino 8) via comandos HTTP (cliques ON/OFF)

**Analogia rápida:** o celular virou um “controle remoto”, a rede virou a “ponte”, e o Arduino executou a ação no mundo real (acender/apagar LED).

***

## 2. Equipamentos utilizados neste laboratório

### 2.1 Placas e módulos

*   **Arduino UNO R3**
*   **Ethernet Shield W5100** (placa Ethernet/shield)

### 2.2 Rede e dispositivos

*   1 roteador (com LAN, WAN e Wi‑Fi)
*   2 cabos de rede (Ethernet)
*   1 computador
*   1 celular (testes via Wi‑Fi)
*   App de celular: **“Ping”** (para pingar o Arduino)

### 2.3 Circuito do LED

*   Protoboard
*   LED vermelho
*   Resistor (usado em série com o LED)
*   Jumpers (cabos)

### 2.4 Componentes do kit (mostrados no início)

*   HC‑SR04 (sensor ultrassônico)
*   DHT11 (temperatura/umidade)
*   LDR (sensor de luz)
*   LEDs e cabos diversos

![Imagem2](https://github.com/user-attachments/assets/a759dd52-e040-4eeb-8bb6-7eb625ef104e)

***

## 3. Topologia da Rede (visão geral)

A rede foi montada para permitir que:

*   o **Arduino** (via Ethernet) e o **computador** (via Ethernet) ficassem na **mesma rede do roteador**
*   o **celular** acessasse o Arduino pela **mesma rede**, via Wi‑Fi
*   o roteador tivesse acesso externo pela **WAN conectada à rede do SENAC**

![Imagem8](https://github.com/user-attachments/assets/a8463f8e-8baa-415a-a5a2-6d76af0c3f13)

***

## 4. Plano de endereçamento IP (DHCP + Reserva)

O Arduino recebeu IP via **DHCP** e depois foi configurada **Reserva de IP** no roteador para o IP não mudar.

> Observação: ao longo do projeto apareceram IPs como **192.168.0.100** (ping) e **192.168.0.101** (Serial Monitor). O essencial é: **o Arduino ficou com IP fixo por reserva DHCP**.

Itens verificados no Serial Monitor:

*   IP
*   Máscara
*   Gateway
*   DNS
 
***

![Imagem9](https://github.com/user-attachments/assets/3333a741-2175-4c88-997c-cd12d5123f1c)

***

![Imagem10](https://github.com/user-attachments/assets/3a327c59-9df0-4fd6-a7fc-216e6601e010)

***

![Imagem11](https://github.com/user-attachments/assets/9c65bee3-43d6-417c-a846-b424be544b7f)

***

## 5. Implementação do Laboratório Real

A seguir está o passo a passo por dia (com códigos e explicações).

***

# **Dia 1 – 11/03/2026**

## 5.1 Preparação e primeiros testes

*   Separação dos componentes e identificação do kit.
*   Arduino plugado no computador (alimentação + envio de código).

***

![Imagem6](https://github.com/user-attachments/assets/a8f9ccd9-1ee3-4f1d-89ff-629df4712951)

***

![Imagem7](https://github.com/user-attachments/assets/a3b26fcf-babc-458f-a6a8-c6a965e97a03)

***

## 5.2 Arduino IDE – interface e primeiro código

Foi aberta a IDE Arduino e explicado que:

*   `setup()` roda **uma vez** (configuração)
*   `loop()` roda **sempre** (repetição)

![Imagem3](https://github.com/user-attachments/assets/eba1e1e5-d79a-4813-8d26-66d5636b71f0)

Primeiro teste (Serial):

```cpp
void setup() {
  Serial.begin(9600);
  Serial.println("Hello World");
}

void loop() {
}
```

Explicação simples:

*   `Serial.begin(9600)` abre o “canal de conversa” Arduino ↔ PC.
*   `Serial.println()` escreve e **pula linha**.

**Analogia:** `setup()` é “arrumar a mochila” antes de sair; `loop()` é “andar” continuamente.

***

## 5.3 Testes no Tinkercad e no Arduino real

*   O código foi testado no **Tinkercad** (simulação) e depois no Arduino real.
*   Depois, o `println()` foi colocado dentro do `loop()` para repetir sem parar.

***

![Imagem4](https://github.com/user-attachments/assets/24b5e11b-5fce-44dd-ae26-64b7521d353d)

***

***

![Imagem5](https://github.com/user-attachments/assets/b99e73ef-f0ca-4171-9bb2-06004af62077)

***

Exemplo (repetição):

```cpp
void setup() {
  Serial.begin(9600);
}

void loop() {
  Serial.println("Hello World");
}
```

Explicação direta:

*   No `loop()`, o Arduino vai imprimir “Hello World” **indefinidamente**.

***

## 5.4 Montagem da rede + obtenção de dados (IP, máscara, gateway, DNS)

Montagem física:

*   Ethernet Shield encaixado no Arduino
*   Arduino no roteador por cabo
*   PC no roteador por cabo
*   WAN do roteador ligada à rede do SENAC

![Imagem8](https://github.com/user-attachments/assets/c3caa75b-d5a4-456a-8c85-c10142020efa)

Código para obter informações de rede:

```cpp
#include <SPI.h>
#include <Ethernet.h>

byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0x00, 0x69 };

void setup() {
  Serial.begin(9600);
  Ethernet.begin(mac);

  Serial.print("IP: ");
  Serial.println(Ethernet.localIP());

  Serial.print("Subnet Mask: ");
  Serial.println(Ethernet.subnetMask());

  Serial.print("Gateway: ");
  Serial.println(Ethernet.gatewayIP());

  Serial.print("DNS: ");
  Serial.println(Ethernet.dnsServerIP());
}

void loop() {}
```

Explicação simples:

*   DHCP dá um IP automaticamente (como receber um “número de casa”).
*   Gateway é a “saída do bairro” (roteador).
*   DNS é a “lista telefônica” da internet.
*   Máscara define o tamanho da rede.

***

![Imagem9](https://github.com/user-attachments/assets/78ae39cc-1349-45cb-b679-e212e944296f)

***

![Imagem10](https://github.com/user-attachments/assets/6ebe7518-d7f8-46cd-ba5f-9b318ac2b1e7)

***

## 5.5 Reserva de IP + Ping

*   Acesso ao painel do roteador
*   Configuração de **Reserva DHCP** (IP fixo para o Arduino)
*   Teste de ping do computador para o Arduino

![Imagem11](https://github.com/user-attachments/assets/488bcffb-b88b-435a-9519-70a331d3ff7d)

Ping funcionando (0% de perda) = comunicação ok.

***

# **Dia 2 – 13/03/2026**

## 5.6 Objetivo do dia

Transformar o Arduino em **Arduino Web Server** e começar a trabalhar com **HTML**, testando no computador e celular.

***

## 5.7 VS Code + Live Server (HTML)

Foi criado/aberto o projeto no VS Code:

*   Pasta do projeto
*   Configurado **salvamento automático**
*   Instalada extensão **Live Server**
*   Criado/alterado `index.html`

![Imagem12](https://github.com/user-attachments/assets/e44c0a9c-762a-4588-aeb2-5cbf7c5614af)

HTML base:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Arduino Web SERVER</title>
</head>
<body>
  <h1>Hello Arduino :)</h1>
</body>
</html>
```

Suas anotações (confirmadas):

*   `<head>` = bastidores (configurações)
*   `<body>` = o que o usuário vê
*   `<title>` = nome da aba e pode aparecer em buscas

Atalhos:

*   VS Code: **Alt + Shift + F** (formata)
*   Arduino IDE: **Ctrl + T** (formata)

***

## 5.8 CSS básico para melhorar a página

Foi adicionado CSS dentro do `<style>`:

![Imagem13](https://github.com/user-attachments/assets/bac50b70-ad45-4665-9535-822803b71108)

```css
<style>
  body {
    font-family: sans-serif;
    text-align: center;
  }
</style>
```

✅ Explicação simples:

*   CSS é “a roupa” do HTML: muda aparência sem mudar a lógica.

***

## 5.9 Início do Web Server no Arduino + IP no Serial Monitor

Código base do servidor na porta 80:

![Imagem14](https://github.com/user-attachments/assets/f1d1e4e8-c989-4430-aa37-919f0b4ba868)

```cpp
#include <SPI.h>
#include <Ethernet.h>

byte mac[] = { 0x00, 0xA2, 0xDA, 0x11, 0x28, 0x99 };
EthernetServer server(80);

void setup() {
  Serial.begin(9600);
  Ethernet.begin(mac);
  server.begin();
  Serial.println("Arduino WEB Server");
  Serial.print("IP: ");
  Serial.println(Ethernet.localIP());
}

void loop() {}
```

✅ Explicação direta:

*   `Ethernet.begin(mac)` pede IP via DHCP.
*   `server.begin()` inicia o servidor.
*   Porta 80 = padrão HTTP.

![Imagem15](https://github.com/user-attachments/assets/1aacf615-97da-4890-9efb-a4be26089a0d)

***

## 5.10 Guardar o HTML dentro do Arduino (PROGMEM + Raw String)

Foi criado o documento HTML dentro do Arduino:

![Imagem16](https://github.com/user-attachments/assets/784d9698-5194-4313-8897-5e71eb616790)

***

![Imagem17](https://github.com/user-attachments/assets/4f22bc33-e6bc-4f51-9817-8dce28053516)

```cpp
const char pagina[] PROGMEM = R"HTML(
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Arduino Web SERVER</title>
</head>
<body>
  <h1>Hello Arduino :)</h1>
</body>
</html>
)HTML";
```

Explicação simples:

*   `PROGMEM` guarda a página na memória de programa (flash), poupando RAM.
*   `R"HTML(... )HTML"` permite colar HTML inteiro sem “quebrar” com aspas/símbolos.

***

## 5.11 `loop()` atendendo navegador e enviando a página

O `loop()` foi programado para responder HTTP:

![Imagem18](https://github.com/user-attachments/assets/2500e358-0d94-442e-9dd5-041b8da37e4d)

Ideia (explicada em blocos):

*   Detecta cliente: `server.available()`
*   Envia cabeçalhos HTTP (200 OK, Content-Type)
*   Envia a página: `client.print(FPSTR(pagina))`
*   Fecha conexão: `client.stop()`

Analogia:

*   navegador “bate na porta”; Arduino “entrega a página” e encerra.

***

## 5.12 Resultado final do Dia 2 (sucesso)

*   IP do Arduino foi digitado no navegador
*   Página abriu no computador e no celular

![Imagem19](https://github.com/user-attachments/assets/70bb0476-492a-48f6-84d9-ea1dba808ad7)

✅ Isso confirmou: servidor OK, rede OK, HTML OK.

***

# **Dia 3 – 16/03/2026**

## 5.13 Objetivo do dia

Controlar um **LED real** pelo site do Arduino (ON/OFF), funcionando no celular via Wi‑Fi.

***

## 5.14 Base do circuito (simulação e documentação)

*   Acesso ao Web Server novamente (montagem da rede repetida)
*   Circuito LED + resistor preparado com base no Tinkercad
*   Anotação do dia: **`<a href>` cria um link** (correto)
![Imagem20](https://github.com/user-attachments/assets/77fb27ed-da0b-47ad-9844-37fe705018cc)

***

![Imagem21](https://github.com/user-attachments/assets/48742b42-c357-4db8-aad9-a40d869ba547)

Depois foi documentado no **Fritzing** (não simula, serve para desenho/documentação):

![Imagem22](https://github.com/user-attachments/assets/dc0be9ff-5c2b-431f-a7c2-beb7e65674a4)

***

## 5.15 Montagem no Arduino real

O circuito foi montado no hardware real:

![Imagem23](https://github.com/user-attachments/assets/e03c6935-8f71-481e-a4a7-94ccf15b4c60)

✅ Explicação direta:

*   resistor limita corrente e protege o LED/Arduino (sem resistor pode queimar).

***

## 5.16 Alterações no HTML: botões ON/OFF

O arquivo HTML do VS Code foi modificado:

*   adicionados controles ON e OFF
*   depois estilizados para parecer botão (melhor no celular)

![Imagem24](https://github.com/user-attachments/assets/404f037f-429c-4a81-86c0-44883b31d206)

***

![Imagem25](https://github.com/user-attachments/assets/639d8c92-5918-4d97-9023-03384e7ce9a1)

Explicação simples do `<a href>`:

*   link vira “comando” enviado ao Arduino.
*   exemplo de comando: `/led-on` e `/led-off` (o caminho que o navegador pede).

***

## 5.17 Substituir HTML antigo no Arduino pelo HTML novo

O código do Arduino Web Server foi reaberto e o HTML antigo foi removido e trocado pelo novo (com botões):

![Imagem26](https://github.com/user-attachments/assets/383fb8ee-5427-4e0f-b01f-8d63a282cdce)

***

![Imagem27](https://github.com/user-attachments/assets/ea9ef2f7-7199-4783-92b5-62bd89f4f3b5)

Foi ajustado o título para **Arduino IoT**:

![Imagem28](https://github.com/user-attachments/assets/b3a19153-e0af-498b-99b4-cc42e25ae199)

***

## 5.18 Alteração no `loop()` para interpretar comandos ON/OFF

O trecho antigo (que lia muito pouco) foi apagado para entrar o código que lê e interpreta o comando:

![Imagem29](https://github.com/user-attachments/assets/220b1ffc-96e9-42db-a7da-c4a51eec88e7)

***

## 5.19 Código novo: ler requisição e controlar o LED

Foi adicionada no `setup()` a configuração do pino:

```cpp
pinMode(led, OUTPUT);
```

Explicação verdadeira:

*   isso define o pino como **saída** (necessário para controlar LED corretamente).
*   brilho depende do circuito (resistor/LED/corrente), mas `OUTPUT` é obrigatório para funcionar certo.

No `loop()`, vocês passaram a ler a requisição e procurar comandos (ex.: `GET /led-on ...`):

![Imagem30](https://github.com/user-attachments/assets/d0b60e0d-4040-4c4f-ad16-ca26ca110e3c)

Lógica do trecho:

```cpp
String request = "";

while (client.available()) {
  char c = client.read();
  request += c;
}

if (request.indexOf("GET /led-on") >= 0) {
  digitalWrite(led, HIGH);
}

if (request.indexOf("GET /led-off") >= 0) {
  digitalWrite(led, LOW);
}
```

Analogia:

*   o Arduino “lê a frase” do navegador e procura uma palavra-chave:
    *   achou “led-on” → liga
    *   achou “led-off” → desliga

Depois disso, foi feito o upload do código para o Arduino.

***

## 5.20 Resultado final do Dia 3 (sucesso)

No site do Arduino (acessado no celular):

*   clicar **ON** → LED acende
*   clicar **OFF** → LED apaga

![Imagem31](https://github.com/user-attachments/assets/72198948-b4bb-4469-a0f3-1ff5eb7a62e8)

Objetivo atingido, especialmente no celular via Wi‑Fi.

***

## 6. Conclusão

Este laboratório permitiu compreender e aplicar, na prática, os principais conceitos de IoT e redes:

*   Montagem de rede local com roteador (LAN/WAN)
*   DHCP e **Reserva de IP** para estabilidade do dispositivo IoT
*   Arduino como **Servidor Web (HTTP)**
*   Construção de interface com **HTML/CSS** pensando em uso no celular
*   Integração completa: **clique no navegador → requisição HTTP → Arduino interpreta → ação no hardware (LED)**

Em resumo: o projeto transformou o Arduino em um dispositivo conectado que **responde a comandos via rede**, que é exatamente a ideia de **Internet das Coisas**.

***
