# formulariobatata

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulário </title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #50C878; 
        }
        .highlight {
            border-color: #38A169; 
        }
        .error {
            border-color: #F56565; 
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen">

    <div class="bg-white shadow-lg rounded-lg p-8 w-full max-w-lg border border-green-500">
        <h1 class="text-2xl font-bold mb-6 text-center text-gray-700">Formulário de Inscrição</h1>
        <form id="formularioInscricao">
            <div class="mb-4">
                <label for="nome" class="block text-sm font-medium text-gray-700">Nome Completo:</label>
                <input type="text" class="mt-1 p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-green-400" id="nome" placeholder="Digite seu nome completo" required>
            </div>

            <div class="mb-4">
                <label for="email" class="block text-sm font-medium text-gray-700">Email:</label>
                <input type="email" class="mt-1 p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-green-400" id="email" placeholder="Digite seu email" required>
            </div>

            <div class="mb-4">
                <label for="telefone" class="block text-sm font-medium text-gray-700">Telefone:</label>
                <input type="tel" class="mt-1 p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-green-400" id="telefone" placeholder="Digite seu telefone" required>
            </div>

            <div class="mb-4">
                <label for="dataNascimento" class="block text-sm font-medium text-gray-700">Data de Nascimento:</label>
                <input type="date" class="mt-1 p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-green-400" id="dataNascimento" required>
            </div>

            <div class="mb-4">
                <label for="genero" class="block text-sm font-medium text-gray-700">Gênero:</label>
                <select class="mt-1 p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-green-400" id="genero" required>
                    <option value="">Selecionar</option>
                    <option value="masculino">Masculino</option>
                    <option value="feminino">Feminino</option>
                    <option value="outro">Outro</option>
                </select>
            </div>

            <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700">Hobbies:</label>
                <div class="flex items-center">
                    <input class="mr-2" type="checkbox" id="hobby1" value="Leitura">
                    <label for="hobby1" class="text-sm text-gray-600">Leitura</label>
                </div>
                <div class="flex items-center">
                    <input class="mr-2" type="checkbox" id="hobby2" value="Esportes">
                    <label for="hobby2" class="text-sm text-gray-600">Esportes</label>
                </div>
                <div class="flex items-center">
                    <input class="mr-2" type="checkbox" id="hobby3" value="Música">
                    <label for="hobby3" class="text-sm text-gray-600">Música</label>
                </div>
            </div>

            <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700">Preferências de Contato:</label>
                <div class="flex items-center mb-2">
                    <input class="mr-2" type="radio" name="contato" id="contatoEmail" value="email" required>
                    <label for="contatoEmail" class="text-sm text-gray-600">Email</label>
                </div>
                <div class="flex items-center mb-2">
                    <input class="mr-2" type="radio" name="contato" id="contatoTelefone" value="telefone" required>
                    <label for="contatoTelefone" class="text-sm text-gray-600">Telefone</label>
                </div>
            </div>

            <div class="mb-4">
                <label for="mensagem" class="block text-sm font-medium text-gray-700">Mensagem:</label>
                <textarea class="mt-1 p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-green-400" id="mensagem" rows="3" placeholder="Escreva sua mensagem aqui..."></textarea>
            </div>

            <button type="submit" class="w-full py-2 bg-green-600 text-white rounded-md hover:bg-green-700 transition duration-200">Enviar</button>
        </form>

        <div id="feedback" class="mt-4 text-center text-green-600 hidden"></div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const formulario = document.getElementById('formularioInscricao');

            
            loadData();

            formulario.addEventListener('submit', function(event) {
                event.preventDefault();
                const isValid = validateForm();
                
                if (isValid) {
                    const feedback = document.getElementById('feedback');
                    feedback.textContent = 'Formulário enviado com sucesso!';
                    feedback.classList.remove('hidden');

                    
                    saveData();
                    
                    
                    formulario.reset();
                }
            });

            
            const inputs = formulario.querySelectorAll('input, select, textarea');

            inputs.forEach(input => {
                input.addEventListener('input', function() {
                    if (this.value) {
                        this.classList.add('highlight');
                        this.classList.remove('error');
                    } else {
                        this.classList.remove('highlight');
                        this.classList.add('error');
                    }
                });
            });

            function validateForm() {
                let valid = true;

                inputs.forEach(input => {
                    if (!input.value) {
                        input.classList.add('error');
                        valid = false;
                    } else {
                        input.classList.remove('error');
                    }
                });

                return valid;
            }

            function saveData() {
                const data = {
                    nome: formulario.nome.value,
                    email: formulario.email.value,
                    telefone: formulario.telefone.value,
                    dataNascimento: formulario.dataNascimento.value,
                    genero: formulario.genero.value,
                    hobbies: [...formulario.querySelectorAll('input[type="checkbox"]:checked')].map(checkbox => checkbox.value),
                    contato: formulario.contato.value,
                    mensagem: formulario.mensagem.value,
                };
                localStorage.setItem('formData', JSON.stringify(data));
            }

            function loadData() {
                const data = JSON.parse(localStorage.getItem('formData'));
                if (data) {
                    formulario.nome.value = data.nome;
                    formulario.email.value = data.email;
                    formulario.telefone.value = data.telefone;
                    formulario.dataNascimento.value = data.dataNascimento;
                    formulario.genero.value = data.genero;
                    data.hobbies.forEach(hobby => {
                        const hobbyCheckbox = document.querySelector(`input[value="${hobby}"]`);
                        if (hobbyCheckbox) {
                            hobbyCheckbox.checked = true;
                        }
                    });
                    formulario.contato.value = data.contato;
                    formulario.mensagem.value = data.mensagem;
                }
            }
        });
    </script>
</body>
</html>
