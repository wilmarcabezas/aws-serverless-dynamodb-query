# aws-serverless-dynamodb-query


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
