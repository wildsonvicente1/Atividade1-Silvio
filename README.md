# Atividade1-Silvio
const express = require('express');
const app = express();
const port = 3000;
app.get('/', (req, res) => {
    res.send('Hello Word!');
});
app.listen(port, () =>{
    console.log('Sevidor rodando em http:localhost:$(port)');
});
