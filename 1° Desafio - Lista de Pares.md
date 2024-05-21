Neste desafio estarei recebendo uma lista de usuário e, será extraído as seguintes informações: 

* Lista de pares;
* Total de conexões e a média;
* pessoas mais conectadas e o maior número de amigos;
* contagem de amigos em comum;
* Interesses similares entre usuários;

~~~python
#Lista de usuários disponibilizada com uma parte dos usuários da DataSiencester:
users = [
    {"id": 0, "name": "Hero"},
    {"id": 1, "name": "Dunn"},
    {"id": 2, "name": "Sue"},
    {"id": 3, "name": "Chi"},
    {"id": 4, "name": "Thor"},
    {"id": 5, "name": "Clive"},
    {"id": 6, "name": "Hicks"},
    {"id": 7, "name": "Devin"},
    {"id": 8, "name": "kate"},
    {"id": 9, "name": "Klein"},
]

# Lista de pares: exemplo - a tupla (0,1) indica que o cientista de dados com a id 0 (Hero) e o
# cientista de dados com a id 1 (Dunn) são amigos;
friendships = [(0, 1), (0, 2), (1, 2), (1, 3), (2, 3), (3, 4), (4, 5), (5, 6), (5, 7), (6, 8), (7, 8), (8, 9) ]
~~~
Já temos os usuários representados como dicionario, vamos aumentá-los com dados extras:
Iremos add uma lista de amigos para cada usuário:

~~~python
for user in users:
    # criando a propriedade friends em uma lista vazia
    user["friends"] = []

# povoando a lista com os dados de friendships:
for i, j in friendships:
    # isso funciona porque users[i] é o usuário cuja id é i
    users[i]["friends"].append(users[j]) # adiciona i como um amigo de j
    users[j]["friends"].append(users[i]) # adiciona j como um amigo de i

#Agora com o dict de cada usuário contendo lista de amigos, podemos obter informações como:

* qual é o número médio de conexões?

  1° Primeiro, encontramos um número total de conexões, resumindo os tamanhos de todas as listas de friends:

 
  def number_of_friends (user):
    return len(user["friends"])

total_connections = sum(number_of_friends(user) for user in users)
print(f'Total de Conexões : {total_connections}')

# atribuindo a variável num_users o total de usuários:
num_users = len(users)

# encontrando a média:
avg_connections = total_connections/num_users

print(f'A media de conexões: {avg_connections}')
~~~

Encontrando as pessoas mais conectadas, são as que possuem o maior número de amigos!

~~~python
# cria uma lista (user_id, number_of_friends)
number_friends_by_id = [(user["id"], number_of_friends(user)) for user in users]

print(f'Pessoas mais conectadas: {number_friends_by_id}')

'Cientistas de Dados Que Você Talvez Conheça'
# primeiro, vamos sugerir um usuário que possa conhecer amigos de amigos: Para cada amigo de um usuário
# itera os amigos daquela pessoa, e coleta todos os resultados

def friends_of_friend_ids_bad(user):
    return [foaf["id"] for friend in user["friends"] for foaf in friend["friends"]]

print('Apresentando amigos de usuários: ')
print(f'Friends User 0 : {[friend["id"] for friend in users[0]["friends"]]}')
print(f'Friends User 1 : {[friend["id"] for friend in users[1]["friends"]]}')
print(f'Friends User 2 : {[friend["id"] for friend in users[2]["friends"]]}')
~~~

Contagem de amigos em comum, utilizando função de ajuda para excluir as pessoas que já são conhecidas do usuário:

~~~python
from collections import Counter # não carregado por padrão

def not_the_same(user, other_user):
    """dois usuários não são os mesmos se possuem ids diferentes"""
    return user["id"] != other_user["id"]

def not_friends(user, other_user):
    """other_user não é um amigo se não está em user[“friends”];"""
    """isso é, se é not_the_same com todas as pessoas em user[“friends”]"""
    return all(not_the_same(friend, other_user) for friend in user["friends"])

def friends_of_friend_ids(user):
    return Counter(foaf["id"]
        for friend in user["friends"]   # para cada um dos meus amigos
        for foaf in friend["friends"]   # que contam *their* amigos
        if not_the_same(user, foaf)     # que não sejam eu
        and not_friends(user, foaf))    # e que não são meus amigos

print(f'Contagem de amigos em comum - User Chi[3] : {friends_of_friend_ids(users[3])}')
print(f'Explicação -> 2 amigos em comum com Hero[0] : 1 amigo em comum com Clive[5]')
~~~

Encontrar usuários com interesses similares: COMPETÊNCIA SIGNIFICATIVA

~~~
interests = [
(0, "Hadoop"), (0, "Big Data"), (0, "HBase"), (0, "Java"),
(0, "Spark"), (0, "Storm"), (0, "Cassandra"),
(1, "NoSQL"), (1, "MongoDB"), (1, "Cassandra"), (1, "HBase"),
(1, "Postgres"), (2, "Python"), (2, "scikit-learn"), (2, "scipy"),
(2, "numpy"), (2, "statsmodels"), (2, "pandas"), (3, "R"), (3, "Python"),
(3, "statistics"), (3, "regression"), (3, "probability"),
(4, "machine learning"), (4, "regression"), (4, "decision trees"),
(4, "libsvm"), (5, "Python"), (5, "R"), (5, "Java"), (5, "C++"),
(5, "Haskell"), (5, "programming languages"), (6, "statistics"),
(6, "probability"), (6, "mathematics"), (6, "theory"),
(7, "machine learning"), (7, "scikit-learn"), (7, "Mahout"),
(7, "neural networks"), (8, "neural networks"), (8, "deep learning"),
(8, "Big Data"), (8, "artificial intelligence"), (9, "Hadoop"),
(9, "Java"), (9, "MapReduce"), (9, "Big Data")
]
~~~

função que encontra usuários com o mesmo interesse:
~~~python
def data_scientists_who_like(target_interest):
    return [user_id for user_id, user_interest in interests if user_interest == target_interest]
~~~

Pensando sobre a possibilidade de termos muitos usuários e interesses, o ideal é contruir um índice de interesses para usuários:

~~~python
from collections import defaultdict
# as chaves são interesses, os valores são listas de user_ids com interests
user_ids_by_interest = defaultdict(list)

for user_id, interest in interests:
    user_ids_by_interest[interest].append(user_id)

# as chaves são user_ids, os valores são as listas de interests para aquele user_id
interests_by_user_id = defaultdict(list)
for user_id, interest in interests:
    interests_by_user_id[user_id].append(interest)
~~~

identificar quem possui interesses em comum com um certo usuário:
-> Itera sobre os interesses do usuário.
-> Para cada interesse, itera sobre os outros usuários com aquele interesse.
-> Mantém a contagem de quantas vezes vemos cada outro usuário

~~~python
def most_common_interests_with(user):
    return Counter(interested_user_id
        for interest in interests_by_user_id[user["id"]]
        for interested_user_id in user_ids_by_interest[interest]
        if interested_user_id != user["id"])
~~~
