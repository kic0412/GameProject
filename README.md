# GameProject

 1. 처음 자바를 배우고 프리메이플식의 게임을 만들 수 있지 않을까 하며 몬스터를 물리치는 형식의 게임을 만들게 되었습니다. 

 - Git-Hub주소 - https://github.com/kic0412/GameProject.git


 # 만들고 싶었던 모티브

<img src="https://github.com/kic0412/GameProject/blob/7c92ba95831f6ee982c2466edfeaf7315697818c/motive.png"  width="600" height="370">


 ## 실제 구현한 모습
 
- 시작 화면
<div>
	<img src="https://github.com/kic0412/GameProject/blob/7c92ba95831f6ee982c2466edfeaf7315697818c/start.PNG"  width="600" height="370">
</div>
<br>
- 선택 화면
<div>
	<img src="https://github.com/kic0412/GameProject/blob/7c92ba95831f6ee982c2466edfeaf7315697818c/select.png"  width="600" height="370">
</div>
<br>
- 실행 화면
<div>
	<img src="https://github.com/kic0412/GameProject/blob/aa03705ab72a8b440423e4b718a7391f8cbe3020/active.PNG"  width="600" height="370">
</div>
<br>

- 공격
<div>
	<img src="https://github.com/kic0412/GameProject/blob/7c92ba95831f6ee982c2466edfeaf7315697818c/attack.PNG"  width="600" height="370"> 
</div>
<br>


## 실행
 ▶실행
 - MAIN클래스를 실행 시키면 됩니다.


## 중요클래스에 대한 설명

 - [Player](#Player)
 - [GameFrame](#GameFrame)
 - [GamePanel](#GamePanel)
 - [Enemy](#Enemy)
 - [Muzan](#Muzan)

## Player
 - 사용자가 플레이하는 플레이어 JLabel클래스, 클래스 내부에는 플레이어가 움직일 수 있는 좌, 우, 점프 등의 메소드가 존재
 - 아직 스프라이트 이미지를 사용하지 않아 공격 모션이 매끄럽지 않음
 - 아래와 같이 움직이는 소스코드가 주된 메소드이다. 맵에 맞췄기 때문에 점프할때의 위치 요소들이 크게 작용 (아래는 이동,공격,점프에 키이벤트를 준 메소드)
``` JAVA
    private void handleKeyEvents() {
        if (keyH.leftPressed && !isLeftWallCrash()) {
            direction = "left";
            moveLeft();
        } else if (keyH.rightPressed && !isRightWallCrash()) {
            direction = "right";
            moveRight();
        }
        if (keyH.spaceBarPressed) {
            direction = "jump";
            up();
        }
        if (keyH.xPressed && !attack) {
            direction = "attack";
            if (this.enemy != null) {
                enemy.decreaseEnemyHp(1,this);
            }
        }
        if (keyH.onePressed && !fire) {
            direction = "fire";
            if(this.enemy != null) {
                enemy.decreaseEnemyHp(5,this);
            }
        }
    }
    
```

## GameFrame
 - 게임에 필요한 각종 패널들과 게임창의 크기 설정을 맡는 클래스 
 ``` JAVA
   public GameFrame() {
        setTitle("코딩의 호흡");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        Dimension screenSize = Toolkit.getDefaultToolkit().getScreenSize();
        int screenWidth = (int) screenSize.getWidth();
        int screenHeight = (int) screenSize.getHeight();
        int frameWidth = 1152;
        int frameHeight = 864;
        int x = (screenWidth - frameWidth) / 2;
        int y = (screenHeight - frameHeight) / 2;

        setBounds(x, y, frameWidth, frameHeight);
        setResizable(false);

        beginningPanel = new BeginningPanel(this);
        selectPanel = new SelectPanel(this);
        creditPanel = new CreditPanel(this);

        setContentPane(beginningPanel);
        setFocusable(true);
        requestFocusInWindow();

        setVisible(true);
    }

 ```

## GamePanel
 - 게임에 필요한 요소들을 구현한 클래스
 - 주요 메서드로는 게임을 실행하는 코드나 player와 enemy의 동작을 업데이트 하거나 이미지를 그려주는 코드가 있다.
 - 아래는 게임을 실행하는 코드이다.
 ``` JAVA
    public void run() {
        double drawInterval = 1000000000 / FPS;
        double delta = 0;
        long lastTime = System.nanoTime();
        long currentTime;
        long timer = 0;
        int drawCount = 0;

        running = true;

        while (gameThread != null && running) {
            currentTime = System.nanoTime();
            delta += (currentTime - lastTime) / drawInterval;
            timer += (currentTime - lastTime);
            lastTime = currentTime;
            if (delta >= 1) {
                update();
                repaint();
                delta--;
                drawCount++;
            }
            if (timer >= 1000000000) {
                drawCount = 0;
                timer = 0;
            }
        }
    }

 ```

## Enemy
 - 몬스터가 상속해야하는 JLable 추상클래스
 - 아래는 몬스터가 상속 받는 속성 값들
``` JAVA
public class Enemy extends JLabel {
    int x, y; // 좌표값
    int speed, jumpSpeed; // 속도값
    protected int hp; //체력
    public int maxHpEnemy = 100;
    public int currentHpEnemy = maxHpEnemy;
    public int hpBarWidthEnemy;
    public int hpBarHeightEnemy;
    private long lastDecreaseTime = 0;
    private boolean isDefeated = false;
    String name;
    GameFrame gameFrame;
    ImageIcon imageicon;
    private GamePanel gamePanel;
    private Player player;
    
```

## Muzan
 - Enemy클래스를 상속받는 몬스터 클래스
 - player를 향해 움직이거나 공격을 하는등의 Muzan을 행동하게 하는 코드가 존재
 - 아래는 player의 좌표를 향해 일해 일정 간격을 두고 이동하는 코드 
 ```JAVA
    public void followCoordinates() {
        if (playerToFollow != null) {
            int targetX = playerToFollow.getX(); // 따라다닐 대상의 X 좌표
            int targetY = playerToFollow.getY(); // 따라다닐 대상의 Y 좌표
            int distanceX = Math.abs(targetX - x);

            // player와 muzan 사이의 거리가 maxDistance 이상일 때만 이동
            if (distanceX > maxDistance) {
                if (x < targetX) {
                    x += speed; // 타겟의 X 좌표를 따라 오른쪽으로 이동
                    direction = "right"; // 이동 방향을 오른쪽으로 설정
                } else if (x > targetX) {
                    x -= speed; // 타겟의 X 좌표를 따라 왼쪽으로 이동
                    direction = "left"; // 이동 방향을 왼쪽으로 설정
                }
                if (y < targetY) {
                    y += speed; // 타겟의 Y 좌표를 따라 아래로 이동
                    direction = "down"; // 이동 방향을 아래로 설정
                } else if (y > targetY) {
                    y -= speed; // 타겟의 Y 좌표를 따라 위로 이동
                    direction = "up"; // 이동 방향을 위로 설정
                }
                movingForward = !movingForward; // 이동 방향이 변경되었으므로 스프라이트 애니메이션 전환을 위한 플래그 업데이트
            }
        }
        autoJump();
    }
```

## 만들어본 후기
 - 자바를 배운지 얼마 안되서 진행한 프로젝트라 제대로 만들 수 있을지 많이 불안했지만
   5명의 팀원과 함께 역할을 나누어 진행하니 어렵지 않게 게임을 만들 수 있었다.
   서로 만든 코드를 합치며 생기는 문제를 확인하고 해결하며 자바에 대한 이해도와 해결능력을
   향상 시키는데 도움이 되었고 자바 언어로 게임을 만들어 본것은 개발자로서의 큰 경험이라고 생각된다.
   

## 부족한점
 - 몬스터의 점프나 공격을 했을 때 플레이어를 향한 것이 아님에도 이상한 움직임이 발생하는등 아직 부족한점이 많다.
