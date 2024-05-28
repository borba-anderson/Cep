<!DOCTYPE html>
<html lang="pt">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consulta de CEP</title>
    <style>
        body {
            font-family: 'Georgia', serif;
            background-color: #fdf5e6;
            color: #333;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            margin: 0;
        }

        form {
            background-color: #fff;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            max-width: 400px;
            width: 100%;
            margin-bottom: 20px;
        }

        h1 {
            font-size: 24px;
            text-align: center;
            margin-bottom: 20px;
            color: #000;
        }

        label {
            font-weight: bold;
            display: block;
            margin-top: 10px;
        }

        input[type="text"] {
            width: calc(100% - 22px);
            padding: 10px;
            margin-top: 5px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 3px;
            background-color: #f8f8f8;
        }

        input[type="button"] {
            padding: 10px 20px;
            background-color: #000;
            color: white;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            font-weight: bold;
        }

        input[type="button"]:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }

        .link-container {
            text-align: center;
        }

        .link-container a {
            text-decoration: none;
            color: #0066cc;
            font-weight: bold;
            font-size: 18px;
        }

        .link-container a:hover {
            text-decoration: underline;
        }
    </style>
</head>

<body>
    <form id="cepForm">
        <h1>Consulta de CEP</h1>
        <label for="cep">CEP</label>
        <input id="cep" type="text" maxlength="9" placeholder="Digite o CEP">

        <label for="bairro">Bairro</label>
        <input type="text" id="bairro" disabled>

        <label for="ddd">DDD</label>
        <input type="text" id="ddd" disabled>

        <label for="ibge">IBGE</label>
        <input type="text" id="ibge" disabled>

        <label for="localidade">Localidade</label>
        <input type="text" id="localidade" disabled>

        <label for="logradouro">Logradouro</label>
        <input type="text" id="logradouro" disabled>

        <label for="uf">UF</label>
        <input type="text" id="uf" disabled>

        <label for="siafi">Siafi</label>
        <input type="text" id="siafi" disabled>

        <input onclick="enviar()" type="button" value="Enviar" id="submitButton">
    </form>

    <div class="link-container">
        <a href="https://exemplo.com/consulta-cep" target="_blank">Visite o site de Consulta de CEP</a>
    </div>

    <script>
        const options = {
            method: "GET",
            mode: "cors",
            cache: "default"
        };

        const cepInput = document.getElementById("cep");
        const submitButton = document.getElementById("submitButton");

        cepInput.addEventListener("blur", () => {
            const cep = cepInput.value.replace(/\D/g, '');
            if (cep.length === 8) {
                buscaCep(cep);
            } else {
                alert("Por favor, insira um CEP válido.");
            }
        });

        function buscaCep(cep) {
            submitButton.disabled = true;
            fetch(`https://viacep.com.br/ws/${cep}/json/`, options)
                .then(response => response.json())
                .then(data => {
                    if (data.erro) {
                        alert("CEP não encontrado.");
                        limpaCampos();
                    } else {
                        preencheCampos(data);
                    }
                })
                .catch(error => {
                    console.error("Erro ao buscar CEP:", error);
                    alert("Ocorreu um erro ao buscar o CEP. Tente novamente.");
                })
                .finally(() => {
                    submitButton.disabled = false;
                });
        }

        function preencheCampos(data) {
            document.getElementById("bairro").value = data.bairro || '';
            document.getElementById("ddd").value = data.ddd || '';
            document.getElementById("ibge").value = data.ibge || '';
            document.getElementById("localidade").value = data.localidade || '';
            document.getElementById("logradouro").value = data.logradouro || '';
            document.getElementById("uf").value = data.uf || '';
            document.getElementById("siafi").value = data.siafi || '';
        }

        function limpaCampos() {
            document.getElementById("bairro").value = '';
            document.getElementById("ddd").value = '';
            document.getElementById("ibge").value = '';
            document.getElementById("localidade").value = '';
            document.getElementById("logradouro").value = '';
            document.getElementById("uf").value = '';
            document.getElementById("siafi").value = '';
        }

        function enviar() {
            const json = {
                "bairro": document.getElementById("bairro").value,
                "ddd": document.getElementById("ddd").value,
                "ibge": document.getElementById("ibge").value,
                "localidade": document.getElementById("localidade").value,
                "logradouro": document.getElementById("logradouro").value,
                "uf": document.getElementById("uf").value,
                "siafi": document.getElementById("siafi").value
            };
            console.log(json);
        }
    </script>
</body>

</html>
