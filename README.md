![GitHub License](https://img.shields.io/github/license/lbruss/lab-redes02)

# **Laboratório de IoT com Arduino – Documentação Parcial (Dias 1 e 2 - 11,13/03/2026)**

**Integrantes do grupo:**

*   Bruss Loza
*   Daniel
*   Ezequiel

***

# **Dia 1 – 11/03/2026**

***

# 1. Objetivo (Parcial)

Iniciar um projeto IoT com Arduino, envolvendo:

*   montagem de rede local
*   configuração de roteador
*   primeiros códigos de comunicação serial
*   utilização do Ethernet Shield
*   obtenção de IP via DHCP
*   testes de comunicação na rede
*   preparação para servidor web

***

# 2. Equipamentos Utilizados

### Placas e Módulos

*   **Arduino UNO R3**\
    → **Aqui se encaixaria melhor a Imagem 7**

*   **Ethernet Shield W5100**\
    → **Aqui se encaixaria melhor a Imagem 6**

### Sensores

*   HC-SR04
*   DHT11
*   LDR

### Acessórios

*   Protoboard
*   LEDs
*   Cabos jumper
*   Cabo USB-B
*   2 Cabos de rede
*   Roteador Wi-Fi

→ **Aqui se encaixaria melhor a Imagem 1 e a Imagem 2**

***

# 3. Primeiros Testes no Arduino IDE

A interface do **Arduino IDE 2.2.6** foi aberta e explicada:

*   `setup()` → roda uma vez
*   `loop()` → roda para sempre

→ **Aqui se encaixaria melhor a Imagem 3**

Primeiro código:

```cpp
void setup() {
  Serial.begin(9600);
  Serial.println("Hello World");
}

void loop() {}
```

***

# 4. Teste no Tinkercad e no Arduino Real

O código foi testado:

*   No simulador Tinkercad
*   No Arduino físico

→ **Aqui se encaixaria melhor a Imagem 4 e a Imagem 5**

O Arduino enviou repetidamente:

    Hello World

***

# 5. Montagem da Rede IoT

A rede foi montada assim:

*   Arduino + Ethernet Shield → Roteador
*   PC → Roteador
*   Roteador → Internet (rede SENAC)

→ **Aqui se encaixaria melhor a Imagem 8**

***

# 6. Código para Obter IP, Máscara, Gateway e DNS

Código executado:

```cpp
Serial.println(Ethernet.localIP());
Serial.println(Ethernet.subnetMask());
Serial.println(Ethernet.gatewayIP());
Serial.println(Ethernet.dnsServerIP());
```

→ **Aqui se encaixaria melhor a Imagem 9**

Saída no Serial Monitor:

→ **Aqui se encaixaria melhor a Imagem 10**

***

# 7. Configuração do Roteador + Ping

Foi feito:

*   Reserva de IP (para o IP do Arduino nunca mudar)
*   Teste de ping via CMD

→ **Aqui se encaixaria melhor a Imagem 11**

O Arduino respondeu com 0% de perda.

***

***

# **Dia 2 – 13/03/2026**

***

# 8. Repetição da Montagem e Configuração

Tudo foi remontado igual ao final do Dia 1:

*   Ethernet Shield conectado
*   Cabos de rede no roteador
*   IP reservado novamente
*   Teste de rede
*   Instalação do app “Ping” no celular

***

# 9. Desenvolvimento do HTML no VS Code

Configurações feitas:

*   Salvamento automático
*   Extensão Live Server instalada
*   Criação de `index.html`

→ **Aqui se encaixaria melhor a Imagem 12**

Código inicial:

```
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arduino WEB SERVER</title>
</head>
<body>
    <h1>Hello Arduino :)</h1>

    <style>
        body {
            font-family: sans-serif;
            text-align: center;
        }
    </style>
</body>
</html>
```

→ **Aqui se encaixaria melhor a Imagem 13**

***

# 10. Programando o Arduino Web Server (Parte 1)

Código inicial no Arduino:

→ **Aqui se encaixaria melhor a Imagem 14**

Explicações:

*   `Ethernet.begin(mac);` → pega IP via DHCP
*   `server.begin();` → inicia servidor web na porta 80
*   `Ethernet.localIP()` → mostra o IP

***

# 11. Serial Monitor Mostrando IP

→ **Aqui se encaixaria melhor a Imagem 15**

O Arduino exibiu:

    Arduino WEB Server
    IP: 192.168.0.101

Servidor funcionando.

***

# 12. Inserindo o HTML dentro do Arduino (PROGMEM)

Criado:

```cpp
const char pagina[] PROGMEM = R"HTML(
HTML AQUI
)HTML";
```

→ **Aqui se encaixaria melhor a Imagem 16**

***

# 13. Colando o HTML dentro do Arduino

Vocês copiaram o HTML do VS Code e colaram dentro do R"HTML()HTML".

→ **Aqui se encaixaria melhor a Imagem 17**

Isso permite ao Arduino enviar a página real para navegadores.

***

# 14. Programando o void loop() (Servidor HTTP Completo)

→ **Aqui se encaixaria melhor a Imagem 18**

Código explicado:

*   `server.available()` → detecta navegador
*   `HTTP/1.1 200 OK` → resposta padrão
*   `Content-Type: text/html` → navegador entende que é HTML
*   `client.print(FPSTR(pagina));` → envia o HTML do PROGMEM
*   `client.stop();` → finaliza conexão

***

# 15. Resultado Final do Dia 2: SUCESSO

→ **Aqui se encaixaria melhor a Imagem 19**

O grupo digitou o IP do Arduino no:

*   computador
*   celular

E a página **Hello Arduino** abriu nos dois dispositivos.

Isso confirma:

* Servidor funcionando\
* HTML funcionando\
* Rede funcionando\
* Arduino como Web Server\
* Código 100% correto

***

# Conclusão Parcial (Dias 1 e 2)

Até agora o grupo:

*   configurou rede IoT
*   programou comunicação serial
*   montou HTML
*   inseriu HTML no Arduino
*   criou servidor web
*   enviou páginas HTML
*   acessou a página pelo navegador

O Arduino agora funciona como um **servidor web completo**, acessível por qualquer dispositivo na mesma rede.

***
