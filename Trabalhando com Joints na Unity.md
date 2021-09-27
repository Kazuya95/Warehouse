# Trabalhando com Joints na Unity

## Aplicando física com Unity 2019.x

[Referência](https://kleberandrade.medium.com/trabalhando-com-joints-na-unity-9eaacc7632c6)

[Joints](https://unity3d.com/pt/learn/tutorials/topics/physics/physics-joints) são utilizados para conectar objetos fisicamente.

- **Hinge Joint:** conecta dois objetos como se eles estivesse ligados por uma dobradiça. Ideal para representar portas, mas também pode ser usado para representar correntes, pêndulos, etc.
- **Spring Joint:** permite a conexão de dois abjetos através da simulação de uma mola. Objetos conectados utilizando esse tipo de joint possuem uma distância máxima de separação que, após soltos, tendem a voltar a sua distância de repouso.
- **Fixed Joint:** permite a conexão entre dois objetos de forma que os movimentos de um objeto sejam dependentes do outro. Similar a utilização das hierarquias de transformação da Unity, porém, implementado através da física. Ideal para objetos que possam ser desconectados um do outro durante a simulação.
- **Configurable Joint:** esse tipo de joint oferece a possibilidade de customização de seu comportamento. Aqui, vários tipos de configuração podem ser efetuadas como restrição de movimento e/ou rotação e aceleração de movimento e rotação. Dessa forma, temos como construir um joint de acordo com a necessidade requerida
- **Character Joint:** usado para criar [Ragdolls](https://www.youtube.com/watch?v=DInV-jHm9rk).

# Projeto da aula

Utilização de Joints para criação de um mini cenário de jogo

![image-20210926155521468](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926155521468.png)



# Preparando um Cenário

- Criar um objeto vazio (CTRL + SHIFT + N)
- Renomear (F2) o objeto para Tile
- Colocar um **Cube** (`Game Object → 3D Object → Cube`**)** como filho e renomear para Grass (grama)



![image-20210926155654289](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926155654289.png)



- Colocar um segundo **Cube** como filho e renomear para Ground (terra)



![image-20210926155725239](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926155725239.png)

- Criar um script chamado `ChangeColor` quer irá gerar um cor aleatório entre duas cores (m_MinColor e m_MaxColor) usando a função [Color.Lerp()](https://docs.unity3d.com/ScriptReference/Color.Lerp.html)



Código em C#:

using UnityEngine;

public class ChangeColor : MonoBehaviour

{

​	public Color m_MinColor = Color.white;

​	public Color m_MaxColor = Color.white;

​	private Renderer m_Renderer;

​	private void Start()

​	{

​	m_Renderer = GetComponent<Renderer>();

​	Color color = Color.Lerp(m_MinColor, m_MaxColor, Random.Range(0.0f, 1.0f));

​	m_Renderer.material.SetColor("_Color", color);

​			}

​	}



- Colocar o script `ChangeColor`na grama (Grass) e configurar as cores minimas e máximas (eu utilizo cores de uma [paleta flat](https://flatuicolors.com/))



![image-20210926160121442](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926160121442.png)



- Colocar o script `ChangeColor`na terra (Ground) e configurar as cores minimas e máximas



![image-20210926160204255](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926160204255.png)



- Criar um **Prefab** do **Tile**
- Formar um cenário, clonando o cubo (CTRL+ D)

![image-20210926160310463](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926160310463.png)

# Criando um Água (Fake)

- Criar um Plane (`GameObject →3D Object →Plane`) e renomear para Water.
- Remover o MeshCollider do Plane
- Corrigir posição e escala conforme figura

![image-20210926160420709](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926160420709.png)

- Criar um Material, colocar o Rendering Mode como Transparent e adicionar uma cor com Alpha (150).



![image-20210926160500710](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926160500710.png)



- Adicionar no Plane (Water)

![image-20210926160613831](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926160613831.png)

- Criar um script chamado `Oscillator`quer irá fazer a água subir e descer

Código C#:

using UnityEngine;

public class Oscillator : MonoBehaviour

{

​	public Vector3 m_Axis = Vector3.up;

​	public float m_Range = 0.3f;

​	public float m_SmoothTime = 2.0f;

​	private Vector3 m_OriginPosition;



​	private void Start()

​	{

​		m_OriginPosition = transform.position;

​	}

​	private void Update()

​	{

​		float movement = m_Range * Mathf.Sin(Time.time * m_SmoothTime);

​		transform.position = m_OriginPosition + m_Axis * movement;

​	}

}

- Adicionar o script `Oscillator`no objeto Water (brinque com os atributos)

# Ponte que balança (hinge)

- Crie um objeto vazio e nomeie-o para **Bridge**

![image-20210926160948224](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926160948224.png)

- Crie um **Cube** (Left Board) filho de **Bridge** e adicione um **Rigibody** nele — marque o IsKinematic para o Cubo não cair

![image-20210926161038580](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161038580.png)

- Crie outro **Cube** (Right Board) filho de **Bridge** e adicione um **Rigibody** nele — marque o IsKinematic para o Cubo não cair

![image-20210926161126934](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161126934.png)

- Clone o **LeftBoard** (mude seu nome para **Board**) e posicione conforme imagem a baixo (X + 1). Desmarque a propriedade IsKinematic do Rigidbody.

![image-20210926161217347](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161217347.png)

- Crie um Cylinder para fazermos a ligação de dois Boards usando 2 HingeJoint.

![image-20210926161308250](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161308250.png)

- Conecte cada HingeJoint do Cylinder em um Board (veja que uma HingeJoint esta conectada (Connected Body) na LeftBoard e a outra na Board. A propriedade **Axis** indica o eixo de rotação e a Anchor indica a posição da Joint.

![image-20210926161406248](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161406248.png)

- Clone o **Cylinder** (CTRL + D) e mova ele para a outra ponta do Board

![image-20210926161449489](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161449489.png)

- Ao dar Play, ficará deste modo

![image-20210926161535404](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161535404.png)

- Clonar a segunda tábua e os dois cilindros até o outro lado.

> Atenção: Conforme for clonando, precisa ligar corretamente os dois cilindros para que eles fiquem conectados nas tabuas corretas.

![image-20210926161625850](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161625850.png)

![image-20210926161707668](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161707668.png)

- Se você quiser, pode colocar o código `ChangeColor`em cada Board

# Ponte levadiça (Spring)

- Crie um Cube e nomeie-o para **Drawbridge**
- Adicione uma **HingeJoint** e posicione conforme figura abaixo

![img](https://miro.medium.com/max/700/1*DePw98jLsnSNsAvuAMaCZA.png)

![image-20210926161840897](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161840897.png)

- Configura a HingeJoint com os seguintes valores

![image-20210926161925646](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926161925646.png)

- Vamos agora criar um script chamado Drawbridge e adicionar nessa ponte levadiça, para subir e descer utilizando a o botão esquerdo do mouse

Código em C#

using UnityEngine;

public class Drawbridge : MonoBehaviour

{

​	public float[] m_Targets = { 90.0f, 0.0f };

​	private int m_Index = 0;

​	private HingeJoint m_Joint;

​	private void Awake()

​	{

​		m_Joint = GetComponent<HingeJoint>();

​	}

​	private void Update()

​	{

​		if (Input.GetButtonDown("Fire1"))

​		{

​			JointSpring spring = m_Joint.spring;

​			m_Index = ++m_Index % m_Targets.Length;

​			spring.targetPosition = m_Targets[m_Index];

​			m_Joint.spring = spring;

​		}

​	}

}

- Agora você pode dar Play e brincar de descer e subir a ponte apertando o botão esquerdo do mouse

# Moinho de vento (Motor)

- Crie um objeto vazio (CTRL + SHIFT + N) e nomeie-o para **Windmill**
- Posicione conforme figura

![image-20210926162314857](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926162314857.png)

- Agora crie um Cube para ser a torre (eu chamei de Base) e coloque como filho do **Windmill**

![image-20210926162354839](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926162354839.png)

- Crie um objeto vazio filho de **Windmill** chamado Motor
- Adicione um **HingeJoint** no Motor e configure da seguinte forma

![image-20210926162438742](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926162438742.png)

- Crie um 2 cubos como filhos de Motor e configure da seguinte forma

![image-20210926162532555](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926162532555.png)

- Para finalizar adicione materiais na **Base** e **Propellers**

![image-20210926162627204](C:\Users\oijkn_000\AppData\Roaming\Typora\typora-user-images\image-20210926162627204.png)

