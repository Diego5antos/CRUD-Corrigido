# CRUD-Corrigido
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CRUD</title>

</head>
<body>
    <h1>CRUD</h1>
    <p>Inicie o menu do programa.</p>

    <script>

    function criarNo(dado) {
    return {
        dado: dado,
        proximo: null
    };
}

function criarLista() {
    return {
        inicio: null
    };
}

function lerDado(mensagem) {
    const entrada = window.prompt(mensagem);

    if (entrada === null) {
        return null;
    }

    const dado = entrada.trim();

    if (dado === "") {
        window.alert("O valor não pode ficar vazio.");
        return null;
    }

    return dado;
}

function lerPosicao(mensagem) {
    const entrada = window.prompt(
        mensagem + "\nAs posições começam em 0."
    );

    if (entrada === null) {
        return null;
    }

    const posicao = Number(entrada.trim());

    if (!Number.isInteger(posicao) || posicao < 0) {
        window.alert("Informe uma posição inteira não negativa.");
        return null;
    }

    return posicao;
}

function listarTodos(lista) {
    const elementos = [];
    let atual = lista.inicio;

    while (atual !== null) {
        elementos.push(atual.dado);
        atual = atual.proximo;
    }

    return elementos;
}

function inserir(lista, dado) {
    const novo = criarNo(dado);

    if (lista.inicio === null) {
        lista.inicio = novo;
        return true;
    }

    let atual = lista.inicio;

    while (atual.proximo !== null) {
        atual = atual.proximo;
    }

    atual.proximo = novo;
    return true;
}

function contarElementos(lista) {
    let quantidade = 0;
    let atual = lista.inicio;

    while (atual !== null) {
        quantidade++;
        atual = atual.proximo;
    }

    return quantidade;
}

function inserirNaPosicao(lista, dado, posicao) {
    if (
        typeof posicao !== "number" ||
        !Number.isInteger(posicao) ||
        posicao < 0
    ) {
        return false;
    }

    if (posicao > contarElementos(lista)) {
        return false;
    }

    const novo = criarNo(dado);

    if (posicao === 0) {
        novo.proximo = lista.inicio;
        lista.inicio = novo;
        return true;
    }

    let anterior = lista.inicio;

    for (let i = 0; i < posicao - 1; i++) {
        anterior = anterior.proximo;
    }

    novo.proximo = anterior.proximo;
    anterior.proximo = novo;

    return true;
}

function formatarLista(lista) {
    const elementos = listarTodos(lista);

    if (elementos.length === 0) {
        return "A lista está vazia.";
    }

    return elementos
        .map(function (dado, posicao) {
            return "[" + posicao + "] " + dado;
        })
        .join("\n");
}

function exibirLista(lista) {
    window.alert(formatarLista(lista));
}

function menu(lista) {
    while (true) {
        const opcao = window.prompt(
            "LISTA LIGADA SIMPLES\n" +
            "[1] Listar todos\n" +
            "[2] Inserir no fim\n" +
            "[3] Inserir na posição\n" +
            "[4] Sair\n" +
            "Digite a opção desejada:"
        );

        if (opcao === null || opcao.trim() === "4") {
            return;
        }

        switch (opcao.trim()) {
            case "1": {
                exibirLista(lista);
                break;
            }

            case "2": {
                const valor = lerDado(
                    "Qual valor deseja inserir no fim?"
                );

                if (valor !== null) {
                    if (inserir(lista, valor)) {
                        window.alert(
                            '"' + valor + '" inserido no fim da lista.'
                        );
                    }
                }

                break;
            }

            case "3": {
                const valor = lerDado(
                    "Qual valor deseja inserir?"
                );

                if (valor === null) {
                    break;
                }

                const posicao = lerPosicao(
                    "Em qual posição deseja inserir?"
                );

                if (posicao === null) {
                    break;
                }

                if (inserirNaPosicao(lista, valor, posicao)) {
                    window.alert(
                        '"' + valor + '" inserido na posição ' +
                        posicao + "."
                    );
                } else {
                    const n = contarElementos(lista);

                    window.alert(
                        "Posição inválida. Com " + n +
                        " elemento(s), use posições de 0 a " + n + "."
                    );
                }

                break;
            }

            default:
                window.alert("Opção inválida.");
        }
    }
}

const lista = criarLista();

const emNavegador =
    typeof window !== "undefined" &&
    typeof window.prompt === "function";

if (emNavegador) {
    menu(lista);
}




    </script>
</body>
</html>

