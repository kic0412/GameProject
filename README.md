# GameProject

 1. 처음 JAVA를 배우고 프리메이플식의 게임을 만들 수 있지 않을까 하며 몬스터를 물리치는 형식의 게임을 만들게 되었습니다. 

 - Git-Hub주소 - https://github.com/kic0412/GameProject.git


 - 만들고 싶었던 모티브

<img src=""  width="600" height="370">


 ## 실제 구현한 모습
 
- 시작 화면
<div>
	<img src=""  width="600" height="370">
</div>
<br>
- 선택 화면
<div>
	<img src=""  width="600" height="370">
</div>
<br>
- 실행 화면
<div>
	<img src=""  width="600" height="370">
</div>
<br>

- 공격
<div>
	<img src=""  width="600" height="370"> 
</div>
<br>



## 실행
 ▶실행
 - MAIN클래스를 실행 시키면 됩니다.



## 중요클래스에 대한 설명

 - [Player](#player)
 - [GameFrame](#GameFrame)
 - [Enemy](#enemy)

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
 - 게임에 필요한 패널로 만든 
 -

 ``` JAVA

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

 


## Enemy클래스를 상속받는 몬스터클래스


## 만들어본 후기
 - 

## 부족한점
 - 


