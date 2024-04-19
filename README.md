# Trabalho Prático01 - Jogo Pacman em MonoGame com C# - Engenharia e Desenvolvimento de Jogos Digitais - Marco Macedo nº27919 / Miguel Freitas nº29562 

# __Indíce__
1. [__Introdução__](#Introdução)
2. [__Implementação__](#Implementação)
3. [__Instruções de Jogo__](#instru)
4. [__Decisões de Implementação__](#decisoes)
5. [__Análise dos Códigos Disponibilizados__](#analise)
6. [__Conclusão__](#Conclusão)

# __Introdução__
Neste trabalho vamos apresentar a implementação de uma versão simplificada do jogo Pac-Man, desenvolvida utilizando o framework MonoGame em C#. O objetivo é fornecer uma visão geral da estrutura do projeto, decisões de implementação e instruções de jogo. Além disso, será feita uma análise dos códigos disponibilizados, abordando a organização e a lógica implementada.
<p align="center">
  <img src="https://s2-techtudo.glbimg.com/Zgdo7fpjWUEzhDFBuMthj2OXnRw=/0x0:695x390/984x0/smart/filters:strip_icc()/i.s3.glbimg.com/v1/AUTH_08fbf48bc0524877943fe86e43087e7a/internal_photos/bs/2021/o/S/uINhTlQ8KW9l4wvqsu4g/2014-10-01-pac-man-curiosidades-classico.jpg"  alt="Pacman" width=700>
</p>

# __Implementação__


## __Estrutura de Pastas:__

* __PacMan/__
    - |-- Content/
    - |-- Fonts/
    - |-- SpriteSheets/
    - |-- Sounds/

* __Code/__
    - [|-- Inky.cs](#4codigos)
    - [|-- Blinky.cs](#4codigos)
    - [|-- Pinky.cs](#4codigos)  
    - [|-- Clyde.cs](#4codigos)
    - [|-- Controller.cs](#controller)
    - [|-- MySounds.cs](#mysounds)
    - [|-- Game1.cs](#game1)
    - [|-- GameOver.cs](#gameover)
    - [|-- Menu.cs](#menu)
    - [|-- Node.cs](#node)
    - [|-- Pathfinding.cs](#pathfinding)
    - [|-- Player.cs](#player)
    - [|-- Program.cs](#program)
    - [|-- Snack.cs](#snack)
    - [|-- SpriteAnimation.cs](#spriteanimation)
    - [|-- Text.cs](#text)
    - [|-- Tile.cs](#tile)
    - [|-- Enemy.cs](#enemy)
    - [|-- SpriteSheet.cs](#spritesheet)
    
    

- __Pacman/:__ Contém recursos como imagens, fontes e sons.
- __Code/:__ Contém o código-fonte do jogo organizado em entidades, gerenciadores, ecras e ajudantes.

<a name="instru"></a>
# __Instruções de Jogo__
* __Objetivo:__ 
    - Mova o Pac-Man pelo labirinto para comer todos os pontos e evitar os fantasmas.
* __Controlos:__
    - __Setas Direcionais:__ Mova o Pac-Man nas direções desejadas.
* __Fantasmas e Vidas:__
    - __Fantasmas:__ Evite ser capturado por os fantasmas.
    - __Vidas:__ Tem um número limitado de vidas.
* __Pontuação:__
    - Acumule pontos ao comer os pontos pelo labirinto.

<a name="decisoes"></a>
# __Decisões de Implementação__
*  __Entidades:__
     - __PacMan:__ Representa o jogador controlável.
     - __Ghost:__ Representa os inimigos que perseguem o Pac-Man.
*  __Gerenciadores:__
     - __GameManager:__ Gerencia o estado geral do jogo, como pontuação e vidas.
*	 __Ecrãs:__
     - __GameScreen:__ Controla o fluxo do jogo, como iniciar, parar e encerrar.
*	 __Ajudantes:__
     - __CollisionHelper:__ Auxilia na detecção de colisões entre entidades.

<a name="analise"></a>
# __Análise dos Códigos Disponibilizados__

<a name="4codigos"></a>
## 	__Inky.cs, Blinky.cs, Pinky.cs, Clyde.cs:__
Cada ficheiro de cada um representa o inimigo no jogo Pac-Man. Ela herda da classe base Enemy e é responsável por inicializar as características específicas do inimigo, como seu alvo de dispersão, retângulos de animação e comportamento de perseguição. 

O código implementa métodos para determinar a posição do alvo de perseguição do inimigo com base na distância em relação ao jogador. Em resumo, esta classe organiza o comportamento e as propriedades do inimigo no contexto do jogo Pac-Man. Como os códigos .cs são semelhantes , concluimos só mostrar um (Blinky.cs):

```
using System.Collections.Generic;
using System.Linq;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace Pacman
{
    public class Blinky : Enemy
    {
        public Blinky(int tileX, int tileY, Tile[,] tileArray) : base(tileX, tileY, tileArray)
        {
            ScatterTargetTile = new Vector2(26, 2);
            type = GhostType.Blinky;

            rectsDown[0] = new Rectangle(1659, 195, 42, 42);
            rectsDown[1] = new Rectangle(1707, 195, 42, 42);

            rectsUp[0] = new Rectangle(1563, 195, 42, 42);
            rectsUp[1] = new Rectangle(1611, 195, 42, 42);

            rectsLeft[0] = new Rectangle(1467, 195, 42, 42);
            rectsLeft[1] = new Rectangle(1515, 195, 42, 42);

            rectsRight[0] = new Rectangle(1371, 195, 42, 42);
            rectsRight[1] = new Rectangle(1419, 195, 42, 42);

            enemyAnim = new SpriteAnimation(0.08f, rectsLeft);
        }
    }
}
```

<a name="controller"></a>
## 	 	__Controller.cs:__
O Controller.cs controla várias partes do jogo, incluindo a criação do mapa, movimento dos personagens, interação com itens no mapa , e o gerenciamento do estado do jogo (normal, fim de jogo, menu). 

Além disso, ele controla a lógica dos fantasmas, a detecção de colisões, e a mudança de estados do jogo, como quando os fantasmas estão em perseguição ou fuga. Em resumo, o código coordena todas as funcionalidades necessárias para proporcionar uma experiência jogável do Pacman.


<a name="mysounds"></a>
## 		__MySounds.cs:__
Este código define uma classe estática chamada MySounds que armazena referências para efeitos sonoros do jogo Pac-Man. 

Ele fornece campos estáticos para cada efeito sonoro, permitindo acesso global e controle centralizado dos sons do jogo. 
Cada campo representa um efeito sonoro específico.
```
using System;
using System.Collections.Generic;
using System.Text;

using Microsoft.Xna.Framework.Audio;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;

namespace Pacman
{
    public static class MySounds
    {
        public static SoundEffect game_start;
        public static SoundEffect munch;
        public static SoundEffectInstance munchInstance;
        public static SoundEffect credit;
        public static SoundEffect death_1;
        public static SoundEffect death_2;
        public static SoundEffect eat_fruit;
        public static SoundEffect eat_ghost;
        public static SoundEffect power_pellet;
        public static SoundEffectInstance power_pellet_instance;
        public static SoundEffect extend;
        public static SoundEffect intermission;
        public static SoundEffect retreating;
        public static SoundEffectInstance retreatingInstance;
        public static SoundEffect siren_1;
        public static SoundEffectInstance siren_1_instance;
        public static SoundEffect siren_2;
        public static SoundEffect siren_3;
        public static SoundEffect siren_4;
        public static SoundEffect siren_5;
    }
}
```
<a name="game1"></a>
## 		__Game1.cs:__
O Game1.cs é o principal do jogo Pac-Man, onde a maioria das operações essenciais acontecem. Controla o ciclo de vida do jogo, desde sua inicialização até a renderização no ecrã. Responsável por carregar recursos, como imagens e sons, também coordena a interação entre os elementos do jogo, como os fantasmas, o jogador e os itens. Além disso, controla o estado do jogo, como paragens, game over e volta ao menu principal. 

Em resumo, é responsável por garantir uma experiência de jogo fluida e envolvente para o jogador.

<a name="gameover"></a>
## 	 	__GameOver.cs:__
A classe GameOver controla o ecrã do fim de jogo. Quando a tecla de espaço é pressionada, volta ao menu principal. 

A mensagem "game over!" é exibida em vermelho, e há uma instrução para pressionar a tecla espaço para voltar ao menu. 

```
using System;
using System.Linq;
using System.Collections.Generic;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using Microsoft.Xna.Framework.Audio;

namespace Pacman
{
    public static class GameOver
    {
        private static SpriteFont basicFont;
        private static Vector2 basicFontPos = new Vector2(93, 400);
        public static SpriteFont setBasicFont
        {
            set { basicFont = value; }
        }

        public static void Update()
        {
            KeyboardState kState = Keyboard.GetState();
            if (kState.IsKeyDown(Keys.Space))
            {
                Game1.gameController.gameState = Controller.GameState.Menu;
            }
        }

        public static void Draw(SpriteBatch spriteBatch, Text text)
        {
            text.draw(spriteBatch, "game over!", new Vector2(100, 321), 48, Text.Color.Red, 2f);
            spriteBatch.DrawString(basicFont, "PRESS SPACE TO GO TO MENU", basicFontPos, Color.Red);
        }
    }
}
```
<a name="menu"></a>
## 	 	__Menu.cs:__
O Menu.cs, responsável por exibir e gerenciar o menu inicial do jogo. Contém métodos para atualizar e desenhar o menu no ecrã, incluindo o logotipo do Pac-Man e a mensagem "PRESS ENTER TO PLAY". 

Quando o jogador clica na tecla Enter, o estado do jogo muda para o modo normal e o som de início do jogo é reproduzido.

```
using System;
using System.Linq;
using System.Collections.Generic;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using Microsoft.Xna.Framework.Audio;

namespace Pacman
{
    public static class Menu
    {
        private static Texture2D pacmanLogo;
        private static Rectangle pacmanLogoPos = new Rectangle(13, 40, 4530/7, 1184/7);
        private static SpriteFont basicFont;
        private static Vector2 basicFontPos = new Vector2(150, 400);

        public static SpriteFont setBasicFont
        {
            set { basicFont = value; }
        }

        public static Texture2D setPacmanLogo
        {
            set { pacmanLogo = value; }
        }

        public static void Update(GameTime gameTime)
        {
            KeyboardState kState = Keyboard.GetState();
            if (kState.IsKeyDown(Keys.Enter))
            {
                Game1.gameController.gameState = Controller.GameState.Normal;
                MySounds.game_start.Play();
            }
        }

        public static void Draw(SpriteBatch spriteBatch)
        {
            spriteBatch.DrawString(basicFont, "PRESS ENTER TO PLAY", basicFontPos, Color.Red);
            spriteBatch.Draw(pacmanLogo, pacmanLogoPos, Color.White);
        }
    }
}
```
<a name="node"></a>
## 	 	__Node.cs:__
A classe Node neste código serve como uma representação abstrata de um ponto específico no mapa do jogo Pac-Man. É essencial para o algoritmo de busca utilizado para calcular os caminhos que os fantasmas devem seguir para alcançar o jogador. Cada nó armazena informações importantes, como a sua posição, se é ou não um local onde os fantasmas podem se mover, os custos associados ao movimento até esse ponto e uma referência ao nó pai. 

Isso permite que o jogo determine os caminhos mais eficientes para os fantasmas percorrerem o labirinto, evitando paredes e obstáculos. Em resumo, a classe Node desempenha um papel crucial na inteligência artificial dos fantasmas, garantindo que eles persigam o jogador de forma inteligente e desafiadora.

```
using System;
using System.Collections.Generic;
using System.Text;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;

namespace Pacman
{
    public class Node
    {
        public int gCost;
        public int hCost;

        public Node parent;
        public Dir ignoreDirection = Dir.None;

        public void setIgnoreDirection(Dir currentDir)
        {
            switch (currentDir)
            {
                case Dir.Right:
                    ignoreDirection = Dir.Left;
                    break;
                case Dir.Left:
                    ignoreDirection = Dir.Right;
                    break;
                case Dir.Down:
                    ignoreDirection = Dir.Up;
                    break;
                case Dir.Up:
                    ignoreDirection = Dir.Down;
                    break;
            }
        }

        public void setParent(Node parent)
        {
            this.parent = parent;
        }
        
        public int fCost {
            get {
                return gCost + hCost;
            }
        }

        public bool isWalkable;

        public Vector2 pos;
        Tile tile;

        public Node(Vector2 pos, Tile[,] tileArray)
        {
            if (pos.X < 0 || pos.Y < 0 || pos.X >= Controller.numberOfTilesX || pos.Y >= Controller.numberOfTilesY)
            {
                this.pos = new Vector2(-100, -100);
            }
            else
            {
                this.pos = pos;
                tile = tileArray[(int)pos.X, (int)pos.Y];
                if (tile.tileType == Tile.TileType.Wall)
                {
                    isWalkable = false;
                }
                else
                {
                    isWalkable = true;
                }
            }

        }

        public Node Copy(Tile[,] tileArray)
        {
            Node node = new Node(pos, tileArray);

            node.hCost = hCost;
            node.gCost = gCost;
            node.ignoreDirection = ignoreDirection;
            if (parent != null)
            {
                node.setParent(parent.Copy(tileArray));
            }

            return node;
        }

        public List<Node> getNeighbours(Tile[,] tileArray)
        {
            List<Node> neighbours = new List<Node>();

            if (ignoreDirection != Dir.Left)
            { 
                Node left = new Node(new Vector2(pos.X - 1, pos.Y), tileArray);
                if (left.pos != new Vector2(-100, -100)) neighbours.Add(left);
            }

            if (ignoreDirection != Dir.Right)
            {
                Node right = new Node(new Vector2(pos.X + 1, pos.Y), tileArray);
                if (right.pos != new Vector2(-100, -100)) neighbours.Add(right);
            }

            if (ignoreDirection != Dir.Up)
            {
                Node up = new Node(new Vector2(pos.X, pos.Y - 1), tileArray);
                if (up.pos != new Vector2(-100, -100)) neighbours.Add(up);
            }

            if (ignoreDirection != Dir.Down)
            {
                Node down = new Node(new Vector2(pos.X, pos.Y + 1), tileArray);
                if (down.pos != new Vector2(-100, -100)) neighbours.Add(down);
            }

            return neighbours;
        }
    }
}
```
<a name="pathfinding"></a>
##  __Pathfinfinding.cs:__
O Pathfinding.cs  encontra o caminho mais curto entre dois pontos no mapa do jogo, considerando os obstáculos e escolhendo o caminho mais eficiente possível. 

Isso é crucial para os inimigos do jogo (fantasmas) planejarem seus movimentos para perseguir o jogador enquanto evitam colisões com paredes e obstáculos.


```
using System;
using System.Linq;
using System.Collections.Generic;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;

namespace Pacman
{
    public class Pathfinding
    {
        public static List<Vector2> findPath(Vector2 startPos, Vector2 endPos, Tile[,] tileArray, Dir currentDirection)
        {
            List<Node> openList = new List<Node>();
            List<Node> closedList = new List<Node>();

            Node startNode = new Node(startPos, tileArray);
            Node endNode = new Node(endPos, tileArray);
            startNode.setIgnoreDirection(currentDirection);
            openList.Add(startNode.Copy(tileArray));

            bool foundPath = false;
            Node currentNode = openList[0].Copy(tileArray);
            while (openList.Count > 0)
            {
                currentNode = openList[0].Copy(tileArray);
                for (int i = 1; i < openList.Count; i++)
                {
                    if (openList[i].fCost < currentNode.fCost || openList[i].fCost == currentNode.fCost && openList[i].hCost < currentNode.hCost)
                    {
                        currentNode = openList[i].Copy(tileArray);
                    }
                }

                deleteNodeOnList(currentNode, openList);
                closedList.Add(currentNode.Copy(tileArray));

                if (currentNode.pos == endNode.pos)
                {
                    foundPath = true;
                    break;
                }

                foreach (Node neighbour in currentNode.getNeighbours(tileArray))
                {
                    if (!neighbour.isWalkable || isNodeInsideList(neighbour, closedList))
                    {
                        continue;
                    }

                    int newMovementCostToNeighbour = currentNode.gCost + getDistance(currentNode, neighbour);
                    if (newMovementCostToNeighbour < neighbour.gCost || !isNodeInsideList(neighbour, openList))
                    {
                        neighbour.gCost = newMovementCostToNeighbour;
                        neighbour.hCost = getDistance(neighbour, endNode);
                        neighbour.setParent(currentNode.Copy(tileArray));

                        if (!isNodeInsideList(neighbour, openList))
                        {
                            openList.Add(neighbour.Copy(tileArray));
                        }
                    }
                }
            }

            if (foundPath)
            {
                List<Vector2> path = new List<Vector2>();
                while(currentNode.pos != startNode.pos)
                {
                    path.Add(currentNode.pos);
                    currentNode = currentNode.parent.Copy(tileArray);
                }
                path.Reverse();
                return path;
            }
            else
            {
                return new List<Vector2>();
            }
        }

        public static void deleteNodeOnList(Node n, List<Node> nodeList)
        {
            for (int i = 0; i < nodeList.Count; i++)
            {
                if (nodeList[i].pos == n.pos)
                {
                    nodeList.RemoveAt(i);
                    break;
                }
            }
        }

        public static bool isNodeInsideList(Node n, List<Node> nodeList)
        {
            foreach (Node node in nodeList)
            {
                if (node.pos == n.pos)
                {
                    return true;
                }
            }
            return false;
        }

        public static int getDistance(Node nodeA, Node nodeB)
        {
            int distX = (int)(nodeA.pos.X - nodeB.pos.X);
            int distY = (int)(nodeA.pos.Y - nodeB.pos.Y);

            if (distX < 0)
                distX *= -1;
            if (distY < 0)
                distY *= -1;

            int totalDist = distX + distY;

            return totalDist;
        }

    }
}
```

<a name="player"></a>
## 	 	__Player.cs:__
O player.cs controla todos os aspectos relacionados ao jogador dentro do jogo. Isso inclui sua posição, direção de movimento, animação, interações com os objetos do ambiente, como comer bolinhas, e a lógica de teletransporte entre os lados do mapa.

No geral o código centraliza todas as operações e comportamentos do jogador dentro do contexto do jogo.


<a name="program"></a>
## 		__Program.cs:__
Este código serve como o ponto de entrada para o jogo Pac-Man.
```
using System;

namespace Pacman
{
    public static class Program
    {
        [STAThread]
        static void Main()
        {
            using (var game = new Game1())
                game.Run();
        }
    }
}
```

<a name="snack"></a>
## 	 	__Snack.cs:__
O snack.cs serve para representar as bolinhas que se comem. Existem dois tipos de bolas: pequenas e grandes, cada um com sua própria pontuação. 

A classe desenha as bolinhas no ecrã e inclui uma lógica de animação para as bolinhas grandes.

```
using System;
using System.Collections.Generic;
using System.Text;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;

namespace Pacman
{
    public class Snack
    {
        public enum SnackType { Small, Big };
        public SnackType snackType;
        private Vector2 gridPosition;
        private int[] gridTile;
        public int scoreGain;
        private Rectangle smallSnackRect = new Rectangle(33, 33, 6, 6);
        private Rectangle bigSnackRect = new Rectangle(24, 72, 24, 24);
        private int radiusOffSet;
        private int timerBigSnack = 20;

        public Vector2 Position
        {
            get { return gridPosition; }
        }

        public Snack(SnackType newSnackType, Vector2 newPosition, int[] newGridTile)
        {
            snackType = newSnackType;
            if (newSnackType == SnackType.Big)
            {
                scoreGain = 50;
                radiusOffSet = 12;
            }
            else
            {
                scoreGain = 10;
                radiusOffSet = 3;
            }
                
            gridPosition = newPosition;
            gridTile = newGridTile;
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            if (snackType == SnackType.Small)
                Game1.spriteSheet1.drawSprite(spriteBatch, smallSnackRect, new Vector2(gridPosition.X + Controller.tileWidth / 2 - radiusOffSet, gridPosition.Y + Controller.tileHeight / 2 - radiusOffSet));
            else
            { 
                if (timerBigSnack >= 10 || Game1.gamePauseTimer > 0)
                    Game1.spriteSheet1.drawSprite(spriteBatch, bigSnackRect, new Vector2(gridPosition.X + Controller.tileWidth / 2 - radiusOffSet, gridPosition.Y + Controller.tileHeight / 2 - radiusOffSet));
                timerBigSnack -= 1;
                if (timerBigSnack < 0)
                {
                    timerBigSnack = 20;
                }
            }
        }
    }
}
```
<a name="spriteanimation"></a>
## 	 	__SpriteAnimation.cs:__
O __SpriteAnimation.cs__ controla a reprodução de retângulos que representam diferentes quadros da animação. 

A classe permite controlar a reprodução, paragem e reinício da animação, além de desenhar os sprites animados no ecrã.
```
using System;
using System.Collections.Generic;
using System.Text;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;

namespace Pacman
{
    public class SpriteAnimation
    {
        private float timer = 0;
        private float threshold;
        private Rectangle[] sourceRectangles;
        private int animationIndex = 0;
        private bool isLooped = true;
        private bool isPlaying;

        public int AnimationIndex
        {
            get { return animationIndex; }
        }

        public bool IsPlaying
        {
            get { return isPlaying; }
            set { isPlaying = value; }
        }

        public SpriteAnimation(float newThreshold, Rectangle[] newSourceRectangles)
        {
            threshold = newThreshold;
            sourceRectangles = newSourceRectangles;
            isPlaying = true;
        }

        public SpriteAnimation(float newThreshold, Rectangle[] newSourceRectangles, int startingAnimIndex)
        {
            threshold = newThreshold;
            sourceRectangles = newSourceRectangles;
            animationIndex = startingAnimIndex;
            isPlaying = true;
        }

        public SpriteAnimation(float newThreshold, Rectangle[] newSourceRectangles, int startingAnimIndex, bool newIsLooped, bool newIsPlaying)
        {
            threshold = newThreshold;
            sourceRectangles = newSourceRectangles;
            animationIndex = startingAnimIndex;
            isLooped = newIsLooped;
            isPlaying = newIsPlaying;
        }

        public void setAnimIndex (int newAnimIndex)
        {
            animationIndex = newAnimIndex;
        }

        public void setSourceRects(Rectangle[] newSourceRects)
        {
            if (newSourceRects.Length != sourceRectangles.Length)
                animationIndex = 0;
            sourceRectangles = newSourceRects;
        }

        public void start()
        {
            isPlaying = true;
            animationIndex = 0;
        }

        public Rectangle[] SourceRectangles
        {
            get { return sourceRectangles; }
        }

        public void Update(GameTime gameTime)
        {
            if (isLooped)
            {
                timer += (float)gameTime.ElapsedGameTime.TotalSeconds;
                if (timer > threshold)
                {
                    timer -= threshold;
                    if (animationIndex < sourceRectangles.Length - 1)
                    {
                        animationIndex++;
                    }
                    else
                    {
                        animationIndex = 0;
                    }
                }
                return;
            }
            // if not looped, plays animation once and then stops (by setting isPlaying to false)
            else
            {
                if (isPlaying)
                {
                    timer += (float)gameTime.ElapsedGameTime.TotalSeconds;
                    if (timer > threshold)
                    {
                        timer -= threshold;
                        if (animationIndex < sourceRectangles.Length - 1)
                        {
                            animationIndex++;
                        }
                        else
                        {
                            isPlaying = false;
                        }
                    }
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch, SpriteSheet spriteSheet, Vector2 position)
        {
            if (isPlaying)
                spriteSheet.drawSprite(spriteBatch, sourceRectangles[animationIndex], position);
        }
    }
}
```
<a name="text"></a>
## 	 	__Text.cs:__
O __Text.cs__ serve para desenhar texto no ecrã do jogo. Ele permite desenhar texto em duas cores diferentes (branco ou vermelho) e ajustar o espaçamento entre as letras. 

As letras disponíveis estão definidas como retângulos em uma folha de sprite. A classe Text fornece métodos para desenhar texto em diferentes escalas e cores.

<a name="tile"></a>
## 	 	__Tile.cs:__
O Tile.cs representa os blocos individuais do mapa do jogo. Cada bloco pode ter um tipo específico, como parede, fantasma, casa de fantasma, jogador ou bola. 

A classe fornece métodos para calcular a distância entre dois blocos e inicializa blocos com posições e tipos específicos.


```
using System;
using System.Collections.Generic;
using System.Text;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;

namespace Pacman
{
    public class Tile
    {
        public enum TileType { None, Wall, Ghost, GhostHouse, Player, Snack };

        public TileType tileType = TileType.None;
        public bool isEmpty = true;

        Vector2 position;

        public Vector2 Position
        {
            get
            {
                return position;
            }
        }

        public static int getDistanceBetweenTiles(Vector2 pos1, Vector2 pos2)
        {
            return (int)Math.Sqrt(Math.Pow(pos1.X - pos2.X, 2) + Math.Pow(pos1.Y - pos2.Y, 2));
        }

        public Tile(Vector2 newPosition)
        {
            position = newPosition;
        }

        public Tile(Vector2 newPosition, TileType newTileType)
        {
            position = newPosition;
            tileType = newTileType;
        }
    }
}
```
<a name="enemy"></a>
## 	 	__Enemy.cs:__
O código define uma série de enums para os tipos de fantasmas (GhostType) e estados do inimigo (EnemyState).Há a definição de diversas propriedades e variáveis para controlar a posição, direção, velocidade e estado dos fantasmas, bem como para armazenar informações sobre as animações dos mesmos.O construtor da classe inicializa várias dessas propriedades com valores padrão e configura as animações dos fantasmas.

Há métodos para desenhar (Draw) e atualizar (Update) os fantasmas no jogo. Isso inclui lógica para decidir a direção dos fantasmas, movê-los pelo tabuleiro e lidar com a colisão deles com o jogador ou com outros elementos do jogo.Existem métodos para calcular a posição alvo dos fantasmas em diferentes estados do jogo, como perseguição, fuga e quando são comidos pelo jogador.
Além disso, há métodos para lidar com eventos específicos, como quando um fantasma é comido pelo jogador. Basicamente esta parte define como os fantasmas se movem, reagem aos eventos do jogo e interagem com o jogador.

<a name="spritesheet"></a>
## __SpriteSheet.cs:__
O SpriteSheet.cs facilita o desenho de sprites em um jogo, permitindo a renderização eficiente de elementos visuais, como personagens e itens. 

Ela gerencia uma imagem que contém vários sprites e fornece métodos para desenhar esses sprites no ecrã em diferentes posições e escalas.

```
using System;
using System.Collections.Generic;
using System.Text;

using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;

namespace Pacman
{
    public class SpriteSheet
    {
        private Texture2D spriteSheet;

        public SpriteSheet(Texture2D newSpriteSheet)
        {
            spriteSheet = newSpriteSheet;
        }

        public void drawSprite(SpriteBatch spriteBatch, Rectangle sourceRectangle, Vector2 position)
        {
            spriteBatch.Draw(spriteSheet, position, sourceRectangle, Color.White);
        }

        public void drawSprite(SpriteBatch spriteBatch, Rectangle sourceRectangle, Vector2 position, float scale)
        {
            spriteBatch.Draw(spriteSheet, position, sourceRectangle, Color.White, 0f, new Vector2(0,0), scale, SpriteEffects.None, 0f);
        }
    }
}
```
# __Conclusão__
Implementamos com sucesso uma versão simplificada do clássico Pac-Man usando MonoGame em C#. Este design recria a emoção do jogo original, mantendo a jogabilidade icónica e desafiante. Com uma jogabilidade simples e objetivos claros, o jogo oferece uma experiência divertida e envolvente para jogadores de todas as idades. A combinação de design retro com tecnologia moderna faz desta versão do Pac-Man um tributo ao legado duradouro deste jogo intemporal.

<p align="center">
  <img src="https://lh4.googleusercontent.com/proxy/Pk0kAWyTCeDVYcgMK34QZbTBDLMRxyOAU_fmoxJUxoJs2Mjge3j6gBT3HY6bIGodMr233c4D1hk-_jPB-P38AJPIYu0QAtIZFPayv_1TlP1cYAYCa2Sr4h8J_yVe" width="300" alt="Pacman">
</p>

