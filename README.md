# extractTimeFromString
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

### extracTimeFromString() `source`:
```javascript
const moment = require("moment");
moment.locale(pt-br);
const ms = require("ms");

async function extractTimeFromString(string) {
  if (!string) throw new Error("Você precisa especificar a string!");
  // Regex para buscar pelo tempo.
  const timePattern = /\d+\s?(days?|hours?|minutes?|dias?|horas?|minutos?|d|h|m)/gi;
  
  // criar um novo RegExp
  const timeRegex = new ReExp(timePattern);
  
  // O método `exec();` procura por um equivalente na string baseado no regex. 
  const result = timeRegex.exec(string);
  
  /* 
  * você também pode definir como:
  * if (!result) return false;
  * sendo assim, ele irá retornar uma boolean com valor false caso não encontre um tempo no texto.
  * mas como eu uso para uma função que necessita TER um texto, eu retorno um erro.
  */
  if (!result) throw new Error("Não há um tempo no texto.");
  
  // regex para buscar por todos os números
  const number = /\d+/g.exec(result[0])[0];
  // regex para buscar por todas as letras
  let type = /[A-Z]+/gi.exec(result[0])[0];
  
  // com essa separação (number e type) teremos um resultado de por exemplo:
  // {number: 14, type: "days" } 
  // 14 dias
  
  const firstIndexPosition = result.index;  
  const lastIndexPosition = (result.index + (number.length + type.length + 1)) 
  
  // Caso seja inserido um "type" em português, transformar em ingês.
  type = type.replace(/dia(s)?/gi, "days");
  type = type.replace(/hora(s)?/gi, "hours");
  type = type.replace(/minuto(s)?/gi, "minutes");

  const miliseconds = new Date().getTime() + ms(number + type);

  return {
    between: [firstIndexPosition, lastIndexPosition],
    ms: miliseconds,
    formated: moment(miliseconds).format("lll")
  }; 
}
```
## Utilizando a função extractTimeFromString
```javascript
extractTimeFromString("!kick <@474407357649256448> dois dias").then(console.log).catch(console.error);
// ∟ console: Não há tempo no texto.

extractTimeFromString("!kick <@474407357649256448> 2dias").then(console.log).catch(console.error);
// ∟ console: {
//  between: [ 28, 34 ],
//  ms: 1582273777448,
//  formated: '21 de Fev de 2020 às 05:29'
// }

extractTimeFromString("!kick <@474407357649256448>     25minutos").then(console.log).catch(console.error);
// ∟ console: {
//  between: [ 32, 42 ],
//  ms: 1582102508349,
//  formated: '19 de Fev de 2020 às 05:55'
// }

extractTimeFromString().then(console.log).catch(console.error);
// ∟ console: Você precisa especificar a string!

extractTimeFromString("!kick <@474407357649256448> 2 horas SPAM/Flood").then(console.log).catch(console.error);
// ∟ console: {
//  between: [ 28, 35 ],
//  ms: 1582108275039,
//  formated: '19 de Fev de 2020 às 07:31'
// }
```

#### Links para auxiliar na compreensão do código:
ordenados com base na aparição dentro da função extractTimeFromString

1. [Moment Docs](https://momentjs.com/docs/) - const moment = require("moment");
2. [ms Docs](https://www.npmjs.com/package/ms) - const ms = require("ms");
3. [Async Function](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/funcoes_assincronas) - async function extractTimeFromString(string) {
4. [RegExp Reference](https://www.w3schools.com/jsref/jsref_obj_regexp.asp) - const timeRegex = new ReExp(timePattern);
5. [exec(); Method](https://www.w3schools.com/jsref/jsref_regexp_exec.asp) - const result = timeRegex.exec(string);
6. [replace(); Method](https://www.w3schools.com/jsref/jsref_replace.asp) - type = type.replace(/dia(s)?/gi, "days");
7. [Date Object](https://www.w3schools.com/js/js_dates.asp) - const miliseconds = new Date().getTime() + ms(number + type);
8. [Array](https://www.w3schools.com/js/js_arrays.asp) - between: [firstIndexPosition, lastIndexPosition],

Obrigado e espero ter ajudado!
Para dúvidas você pode me chamar no discord! Eleven_#8917
