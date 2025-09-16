Sistema de Consultas CPX
Este é um sistema de consultas para fins de roleplaying, permitindo que os membros de uma corporação policial gerenciem e consultem registros de cidadãos, civis e registrem atividades. O sistema é totalmente integrado com o Firebase para armazenamento de dados em tempo real e possui diferentes níveis de acesso para simular a hierarquia de uma força policial.

Funcionalidades Principais
Sistema de Login: Acesso restrito com diferentes níveis de permissão.

Hierarquia de Acesso:

Alto Escalão: Ver, cadastrar, editar e excluir registros.

Oficial: Ver e cadastrar novos registros.

Praça: Apenas visualizar registros.

Cadastro de Membros da Corporação: Formulário dedicado para adicionar novos membros com nome, RG, área da polícia, nível de acesso e senha.

Cadastro de Civis: Formulário para registrar civis com nome, RG, CNH, porte de arma, status e localização.

Consulta de Cidadãos: Uma lista completa com todos os registros, incluindo a possibilidade de ver detalhes.

Registro de Atividades: Um log em tempo real de todas as ações importantes no sistema, como cadastros e edições.

Como Configurar e Hospedar
Para que o sistema funcione corretamente, você precisa conectá-lo a um projeto no Firebase. Siga estes passos:

Crie um projeto no Firebase:

Acesse o Console do Firebase.

Clique em Add project e siga as instruções para criar um novo.

Dentro do seu projeto, adicione um aplicativo Web.

Configure o Firestore Database e o Storage:

No menu lateral, vá para Firestore Database e clique em Create database. Selecione o modo de production para garantir a segurança dos dados.

Vá para Storage no menu lateral e clique em Get started.

Ajuste as Regras de Segurança:

Vá em Firestore Database -> Rules e substitua as regras padrão pelo código abaixo para permitir que os usuários façam login:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/{appId}/{doc=**} {
      allow read, write: if request.auth != null;
    }
  }
}

Em Storage -> Rules, substitua as regras padrão pelo código abaixo para permitir o upload de imagens:

rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /artifacts/{appId}/uploads/{userId}/{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}

Habilite a Autenticação Anônima:

No menu lateral, vá para Authentication -> Sign-in method.

Habilite a opção Anonymous para que o site possa se conectar ao banco de dados.

Adicione as Contas de Usuário:

No seu console do Firebase, vá para Firestore Database -> Data.

Crie a seguinte estrutura de coleções e documentos para os usuários:
/artifacts/{ID do seu projeto}/public/data/users

Dentro da coleção users, adicione um documento para cada conta, como admin, oficial1 ou soldado. Cada documento deve ter os campos username, password e role com os valores correspondentes.

Faça o Deploy do Site:

Use o Netlify (ou outro serviço de hospedagem) e faça o upload do arquivo index.html.

Com a sua configuração do Firebase correta, o site estará no ar e pronto para uso.

Créditos
Desenvolvedor: Kaleb0
