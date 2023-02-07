## API de carros usados

1. Usuários podem se cadastrar
2. Usuários podem ter uma estimativa de quanto seu carro vale baseado em alguns parâmetros
3. Usuários podem reportar o valor de venda dos carros
4. Admins devem aprovar as vendas reportadas

Algumas rotas da API:

1. POST auth/signup - {email, senha} - cria um usuário e faz login
2. POST auth/signin - {email, senha} - faz login
3. GET reports - QueryString (marca, modelo, ano, quilometragem, latitude e longitude) - recebe a estimativa
4. POST reports - {marca, modelo, ano, quilometragem, latitude, longitude, valor} - reporta o valor de venda do carro
5. PATCH reports - {aprovado} - Aprova ou rejeita um relatório

Módulos

1. users - gerenciamento de usuários
2. reports - gerenciamento de relatórios
