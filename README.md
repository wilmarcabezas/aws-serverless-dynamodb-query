# AWS Serverless con DynamoDB y Node.js 

Este repositorio contiene un ejemplo de cómo crear una aplicación serverless usando AWS Lambda, DynamoDB y Node.js.

## ¿Qué hace esta aplicación?
Esta aplicación permite a los usuarios recuperar información sobre los jugadores de un juego específico almacenado en una tabla de DynamoDB mediante una solicitud GET.

## Tecnologías utilizadas
- [AWS Lambda](https://aws.amazon.com/lambda/)
- [AWS DynamoDB](https://aws.amazon.com/dynamodb/)
- [Node.js](https://nodejs.org/)

## Instrucciones de instalación
1. Clona este repositorio en tu computadora: git clone https://github.com/wilmarcabezas/aws-serverless-dynamodb-query.git
2. Instala las dependencias necesarias ejecutando el siguiente comando en la raíz del proyecto: npm install
3. Crea una tabla en DynamoDB con el nombre especificado en el archivo `serverless.yml`.
4. Configura las credenciales de AWS en tu computadora.
5. Ejecuta el siguiente comando para desplegar la aplicación en tu cuenta de AWS: serverless deploy
6. Copia el endpoint generado al ejecutar el comando anterior y pegalo en tu navegador para probar la aplicación.

🚀 ¡Listo! Ahora tienes una aplicación serverless corriendo en AWS.




# Funcion de consulta

```` js
const Responses = require('../common/API_Responses');
const Dynamo = require('../common/Dynamo');

const { withHooks } = require('../common/hooks');

const tableName = process.env.tableName;

const handler = async event => {
    if (!event.pathParameters.game) {
        // failed without a game
        return Responses._400({ message: 'missing the game from the path' });
    }

    const game = event.pathParameters.game;

    const gamePlayers = await Dynamo.query({
        tableName,
        index: 'game-index',
        queryKey: 'game',
        queryValue: game,
    });

    return Responses._200(gamePlayers);
};

exports.handler = withHooks(handler);


````
Esta función es un manejador de AWS Lambda que se utiliza para recuperar información sobre los jugadores de un juego específico almacenado en una tabla de DynamoDB.

* Primero, se verifica si se proporcionó un parámetro de ruta "game" en el evento. Si no es así, se devuelve una respuesta HTTP 400 con el mensaje "falta el juego en el camino".

* Si se proporcionó el parámetro "game", se almacena en una variable llamada "game".

* Luego, se utiliza el módulo Dynamo para realizar una consulta en la tabla DynamoDB especificada en la variable "tableName" con un índice "game-index" y un valor de búsqueda "game" (el valor proporcionado en el parámetro de ruta). La consulta devuelve información sobre los jugadores del juego especificado.

* Finalmente, se devuelve una respuesta HTTP 200 con la información recuperada sobre los jugadores del juego.

La función también utiliza la función "withHooks" del módulo "../common/hooks" para ejecutar código adicional antes o después del manejador.
