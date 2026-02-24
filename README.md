<!DOCTYPE html>
<html lang="pt-br" ng-app="app">
<head>
    <meta charset="UTF-8">
    <title>Carrinho AngularJS</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
    <style>
        table { border-collapse: collapse; width: 70%; }
        th, td { border: 1px solid #f20f0f; padding: 8px; text-align: center; }
        input { width: 80px; }
        button { cursor: pointer; }
    </style>
</head>
<body ng-controller="CarrinhoCtrl">

<h2>Produtos</h2>

<button ng-click="adicionar(prod)" ng-repeat="prod in produtos">
    {{ prod.nome }} - {{ prod.preco | currency:'R$ ' }} / Kg
</button>

<h2>Carrinho</h2>

<table>
    <thead>
        <tr>
            <th>Produto</th>
            <th>Qtd (Kg)</th>
            <th>Preço</th>
            <th>Total</th>
            <th>Ação</th>
        </tr>
    </thead>
    <tbody>
        <tr ng-repeat="item in carrinho">
            <td>{{ item.nome }}</td>
            <td><input type="number" step="0.001" ng-model="item.qtd"></td>
            <td>{{ item.preco | currency:'R$ ' }}</td>
            <td>{{ item.preco * item.qtd | currency:'R$ ' }}</td>
            <td><button ng-click="remover($index)">Remover</button></td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <th colspan="3">Total Geral</th>
            <th>{{ total() | currency:'R$ ' }}</th>
            <th></th>
        </tr>
    </tfoot>
</table>

<script>
angular.module('app', [])
.controller('CarrinhoCtrl', function ($scope) {

    $scope.produtos = [
        { id: 1, nome: 'Carne Bovina', preco: 42.90 },
        { id: 2, nome: 'Frango', preco: 18.00 },
        { id: 3, nome: 'Linguiça', preco: 26.50 }
    ];

    $scope.carrinho = [];

    $scope.adicionar = function (prod) {

        let qtd = parseFloat(prompt('Qtd (Kg):', 1));
        if (!qtd || qtd <= 0) return;

        let item = $scope.carrinho.find(i => i.id === prod.id);

        if (item) {
            item.qtd += qtd;
        } else {
            $scope.carrinho.push({
                id: prod.id,
                nome: prod.nome,
                preco: prod.preco,
                qtd: qtd
            });
        }
    };

    $scope.remover = function (index) {
        $scope.carrinho.splice(index, 1);
    };

    $scope.total = function () {
        return $scope.carrinho.reduce((s, i) => s + (i.preco * i.qtd), 0);
    };
});
</script>
<h3>JSON do Carrinho</h3>
<pre>{{ carrinho | json }}</pre>
</body>
</html>
