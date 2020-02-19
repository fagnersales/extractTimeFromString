# msFormater
Busque uma data inserida dentro do texto com facilidade!

## Packages necessários:
- [npmjs/ms](https://www.npmjs.com/package/ms) para transformar a string em milisegundos.
```javascript
npm install ms
```
- [npmjs/moment](https://www.npmjs.com/package/moment) para formatar os milisegundos em uma data.
```javascript
npm install moment
```

## Função a ser utilizada:
```javascript
async function extractTimeFromString(string) {
  if (!string) throw new Error("Você precisa especificar a string!");
  
  const timePattern = /\d+\s?(days?|hours?|minutes?|dias?|horas?|minutos?|d|h|m)/gi;
  const timeRegex = new ReExp(timePattern);
  
  const result = timeRegex.exec(string);
  
  /* você também pode definir como:
  * if (!result) return false;
  * sendo assim, ele irá retornar uma boolean com valor falso caso não encontre um tempo no texto.
  * mas como eu uso para uma função que necessita TER um texto, eu retorno um erro.
  */
  if (!result) throw new Error("Não há um tempo no texto.");

  const number = /\d+/g.test(result[0])[0];
  
  let type = /[A-Z]+/gi.exec(result[0])[0];
  
  type = type.replace(/dia(s)?/gi, "days");
  type = type.replace(/hora(s)?/gi, "hours");
  type = type.replace(/minuto(s)?/gi, "minutes");

  const miliseconds = new Date().getTime() + ms(number + type);

  return {
    ms: miliseconds,
    formated: moment(miliseconds).format("lll")
  }; 
}


}
```
## Retornos com base na função acima:
```
extractTimeFromString("Estou aqui a dois dias!").then(console.log).catch(console.error);
∟ console: Erro: Não há tempo no texto.
extractTimeFromString().then(console.log).catch(console.error);
extractTimeFromString().then(console.log).catch(console.error);
extractTimeFromString().then(console.log).catch(console.error);
extractTimeFromString().then(console.log).catch(console.error);
```
