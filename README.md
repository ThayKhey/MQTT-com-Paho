
## Descrição

Este projeto utiliza a biblioteca **Paho MQTT** para se conectar a um **broker MQTT** e fazer o **subscribe** em um tópico específico. O programa se conecta ao broker, aguarda e recebe mensagens enviadas para o tópico configurado. Quando uma mensagem é recebida, o sistema imprime o conteúdo da mensagem no terminal.

## Requisitos

- Python 3.x
- Biblioteca **Paho MQTT** instalada:
  - Você pode instalar a biblioteca com o comando:
    ```bash
    pip install paho-mqtt
    ```
- Acesso a um **broker MQTT** (público ou privado) para testar a conexão.

## Funcionalidade

1. **Conexão ao Broker**: O cliente MQTT se conecta ao broker MQTT especificado.
2. **Subscribe no Tópico**: O cliente automaticamente faz o **subscribe** no tópico `/TEF/device001/attrs/p` assim que a conexão ao broker é bem-sucedida.
3. **Recebimento de Mensagens**: Quando uma mensagem é recebida no tópico, ela é exibida no console com o conteúdo da mensagem e o tópico.

## Como Funciona

1. **Conectar ao Broker MQTT**:
   - O código se conecta a um broker MQTT com o endereço e porta definidos nas variáveis `Broker` e `PortaBroker`.
   
2. **Callbacks**:
   - **on_connect**: Chamado quando a conexão ao broker é estabelecida. Ele se inscreve automaticamente no tópico configurado.
   - **on_message**: Chamado quando uma mensagem é recebida no tópico assinado. A mensagem e o tópico são exibidos no console.

3. **Loop de Escuta**: O método `loop_forever()` mantém o cliente MQTT em execução, aguardando e processando mensagens.

## Como Usar

1. **Configurar o Broker**:
   - Substitua `{{url}}` pela URL do seu broker MQTT.
   
2. **Executar o Script**:
   - Execute o script Python para iniciar o cliente MQTT:
     ```bash
     python nome_do_arquivo.py
     ```
   
3. **Verificar as Mensagens**:
   - Após a execução, qualquer mensagem recebida no tópico `/TEF/device001/attrs/p` será exibida no console.

## Código

```python
import paho.mqtt.client as mqtt
import sys

# Definições
Broker = "{{url}}"  # URL do broker MQTT
PortaBroker = 1883  # Porta do broker
KeepAliveBroker = 60  # Tempo de keep alive
TopicoSubscribe = "/TEF/device001/attrs/p"  # Tópico para se inscrever

# Callback - Conexão ao broker realizada
def on_connect(client, userdata, flags, rc):
    print("[STATUS] Conectado ao Broker. Resultado de conexão: " + str(rc))
    # Faz subscribe automático no tópico
    client.subscribe(TopicoSubscribe)

# Callback - Mensagem recebida do broker
def on_message(client, userdata, msg):
    MensagemRecebida = str(msg.payload)
    print("[MSG RECEBIDA] Tópico: " + msg.topic + " / Mensagem: " + MensagemRecebida)

# Programa principal
print("[STATUS] Inicializando MQTT...")
# Inicializa MQTT
client = mqtt.Client(mqtt.CallbackAPIVersion.VERSION1)
client.on_connect = on_connect
client.on_message = on_message

client.connect(Broker, PortaBroker, KeepAliveBroker)
client.loop_forever()
```

## Observações

- **Broker**: Certifique-se de que a URL do broker e a porta estejam corretas para garantir que a conexão funcione.
- **Tópico**: O código está configurado para se inscrever no tópico `/TEF/device001/attrs/p`. Caso precise de outro tópico, altere o valor da variável `TopicoSubscribe`.
- **Mensagens**: As mensagens são exibidas no formato bruto, ou seja, como bytes. Caso queira um formato específico, pode ser necessário realizar o processamento adicional.

## Possíveis Melhorias

- **Persistência de Conexão**: Implementar reconexão automática caso a conexão ao broker seja perdida.
- **Processamento de Mensagens**: Adicionar lógica para processar ou armazenar as mensagens recebidas em uma base de dados ou em arquivos.

--- 
