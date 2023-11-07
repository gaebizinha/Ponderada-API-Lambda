# Ponderada-API-Lambda

Para utilizar é preciso ter o thunder clients no VSCode.

## Endpoint - [https://sowvw2n9ee.execute-api.us-east-1.amazonaws.com/default/TestePonderada1](https://sowvw2n9ee.execute-api.us-east-1.amazonaws.com/default/TestePonderada1)

```python
import json
import base64

username = "gabi"
password = "1510"

def lambda_handler(event, context):
    if 'headers' in event and 'Authorization' in event['headers']:
        encoded_credentials = event['headers']['Authorization'].split(' ')[-1]
        decoded_credentials = base64.b64decode(encoded_credentials).decode('utf-8')
        user, pwd = decoded_credentials.split(':')

        if user == username and pwd == password:
            
            return {
                'statusCode': 200,
                'body': json.dumps(event['body'])
            }
    
    return {
        'statusCode': 401,
        'body': json.dumps({'error': 'Acesso não autorizado'})
    }
```

1. `import json`:
    - O módulo `json` é usado para criar a resposta HTTP com o corpo no formato JSON.
2. `import base64`:
    - Importa o módulo `base64`, que é usado para decodificar a string codificada em Base64 que representa as credenciais no cabeçalho "Authorization". É necessário para autenticação Basic.
3. `username` e `password`:
    - Definição do nome de usuário (`username`) e a senha (`password`) que serão usados para autenticação.
4. `def lambda_handler(event, context):`:
    - Esta linha define a função Lambda. Ela recebe dois parâmetros: `event` e `context`. O parâmetro `event` contém informações sobre a solicitação HTTP recebida, incluindo o cabeçalho "Authorization" e o corpo da solicitação.
5. `if 'headers' in event and 'Authorization' in event['headers']:`:
    - Primeiro tem é verificado se o objeto `event` contém um campo 'headers' e se esse campo contém um campo 'Authorization'. O cabeçalho "Authorization" é onde as credenciais de autenticação são transmitidas.
6. `if user == username and pwd == password:`:
    - Verifica se o nome de usuário e a senha fornecidos na solicitação correspondem aos valores configurados nas variáveis `username` e `password`. Se as credenciais estiverem corretas, a solicitação é considerada autenticada.
7. `return {'statusCode': 200, 'body': json.dumps(event['body'])}`:
    - Se as credenciais estiverem corretas, o código retorna uma resposta com um status 200 (OK) e o corpo da solicitação em formato JSON.
8. `return {'statusCode': 401, 'body': json.dumps({'error': 'Acesso não autorizado'})}`:
    - Se as credenciais estiverem ausentes ou incorretas, o código retorna uma resposta com um status 401 (Acesso não autorizado) e uma mensagem de erro em formato JSON. Isso indica que a autenticação falhou.
