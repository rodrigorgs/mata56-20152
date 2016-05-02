---
layout: lisp
title:  "Revisão de programação funcional: respostas"
date:   2016-05-02 16:40:00 -0300
categories: aula
---

# Questão 1

```javascript
function calculaMediaTurma(dados, turma) {
  // --- aqui vem sua implementação
  var dadosDaTurma = R.filter(x => x.turma == turma,
                              dados);
  var qtdAlunos = dadosDaTurma.length;
  if (qtdAlunos == 0) {
    return 0;
  } else {
    var somaNotas = R.reduce(
        (soma, aluno) => soma + aluno.nota,
        0,
        dadosDaTurma);
    return somaNotas / qtdAlunos;
  }
  // ---
}
// testes
teste(8, calculaMediaTurma([{nome: "A", turma: "1", nota: 8}], "1"));
// --- acrescente aqui os seus testes
teste(0, calculaMediaTurma([]));
teste(7, calculaMediaTurma([
      {nome: "A", turma: "1", nota: 6},
      {nome: "C", turma: "2", nota: 7}],
  "2"));
teste(7, calculaMediaTurma([
      {nome: "A", turma: "1", nota: 6},
      {nome: "B", turma: "1", nota: 8},
      {nome: "C", turma: "2", nota: 9}],
  "1"));
```

# Questão 2

```javascript
function contaPred(pred, lista) {
  // --- aqui vem sua implementação
  if (lista.length == 0) {
    return 0;
  } else if (lista[0] instanceof Array) {
    return contaPred(pred, lista[0]) +
      contaPred(pred, lista.slice(1));
  } else if (pred(lista[0])) {
    return 1 + contaPred(pred, lista.slice(1));
  } else {
    return contaPred(pred, lista.slice(1));
  }
  // ---
}
// testes
teste(3, contaPred(x => x % 2 == 1, [1, [6, 3, [5]]]));
// --- escreva pelo menos mais dois casos de teste
teste(0, contaPred(x => x == 1, []));
teste(2, contaPred(x => x == 1, [1, 1]));
teste(1, contaPred(x => x == 1, [2, 1, 2]));
teste(1, contaPred(x => x == 1, [2, [4, 1], 2]));
teste(0, contaPred(x => x == 1, [2, [4, 0], 2]));
```

# Questão 3

```lisp
; --- escreva abaixo a função
(defun soma-aux (lista, acum)
  (cond
    ((null lista) acum)
    (t (soma-aux (cdr lista) (+ acum (car lista))))))
(defun soma (lista)
  (soma-aux lista 0))
; --- testes
(teste 6 (soma '(1 2 3)))
; --- escreva abaixo pelo menos 2 testes adicionais
(teste 0 (soma '()))
(teste 3.5 (soma '(1.5 2)))
```

# Questão 4

```javascript
function chamaEmpolgadamente(funcao) {
  return funcao() + "!!!";
}
function cumprimento(nome) {
  return "Alo, " + nome;
}
function main(nome) {
  return chamaEmpolgadamente(
  /* --- modifique esta função */
    () => cumprimento(nome)
    // ou function () { return cumprimento(nome); }
  /* --- */
  );
}
// testes
teste("Alo, mundo!!!", main("mundo"));
// --- adicione pelo menos 2 testes
teste("Alo, universo!!!", main("universo"));
teste("Alo, !!!", main(""));
```

# Questão 5

```javascript
var maximo = R.pipe(
/* --- complete o codigo */
  function (lista) {
    return R.filter(elem => elem % 2 == 1,
                    lista);
  },
  function (listaImpares) { 
    return R.reduce(
      (max, elem) => elem > max ? elem : max,
      -999,
      listaImpares);
  }
/* --- */
);
// testes
teste(3, maximo([1, 2, 3, 4]));
teste(-999, maximo([6, 2, 8, 4]));
teste(-999, maximo([]));
teste(1, maximo([-3, 2, 1]));
teste(3, maximo([3, 2, 1]));
teste(3, maximo([1, 2, 3]));
```

----

Alternativa:

```javascript
var maximo = R.pipe(
/* --- complete o codigo */
  R.filter(elem => elem % 2 == 1),
  R.reduce((max, elem) => elem > max ? elem : max,
      -999)
/* --- */
);
// testes
teste(3, maximo([1, 2, 3, 4]));
teste(-999, maximo([6, 2, 8, 4]));
teste(-999, maximo([]));
teste(1, maximo([-3, 2, 1]));
teste(3, maximo([3, 2, 1]));
teste(3, maximo([1, 2, 3]));
```

---

Alternativa 2:

```javascript
function filtraImpar(lista) {
  return R.filter(elem => elem % 2 == 1,
                    lista);
}
function maximo(lista) {
  return R.reduce(
      (max, elem) => elem > max ? elem : max,
      -999,
      lista);
}

var maximo = R.pipe(
/* --- complete o codigo */
  filtraImpar,
  maximo
/* --- */
);
// testes
teste(3, maximo([1, 2, 3, 4]));
teste(-999, maximo([6, 2, 8, 4]));
teste(-999, maximo([]));
teste(1, maximo([-3, 2, 1]));
teste(3, maximo([3, 2, 1]));
teste(3, maximo([1, 2, 3]));
```
