# Bubble Pop

흘러나오는 음악에 맞춰 버튼을 누르는 흔한 리듬게임이다.

하지만 조금 다른 방식의 스타일로 진행되는 게임에 새로움을 느낄 수 있다.

최고 점수를 기록하여 사용자의 승부욕을 고취시킬 수 있다.

<img src="E:\_00_java_project\window1.PNG" style="zoom: 25%;" />

<img src="E:\_00_java_project\window2.PNG" style="zoom:25%;" />

<img src="E:\_00_java_project\window3.PNG" style="zoom:25%;" />



## 개발환경

> JDK 11.0.9
> IDE : Eclipse
> DB : MariaDB 10.5.6



## 주요기능

- 키보드로 조작
- 남은 기회 소멸 시 게임 오버
- 일시정지 후 재시작 및 초기화면 이동
- 점수대별 최종 성적 표시 (SS, S, A, B, C, D, F)
- 점수 DB에 입력
- DB에 있는 최고 점수 추출



## 프로그램의 특징

- 여타 리듬게임과는 다른 방식으로 만들고자 하여 Note가 위에서 아래로 내려와 특정 위치에서 반정하는 방식이 아닌, 원 형태의 Note가 설정한 속도로 줄어들어 특정 지름에서 판정하는 방식으로 만들었다.

- 원 형태의 Note를 어떻게 그릴 지 많이 고심했다. Graphics 클래스와 Timer를 사용해 줄어드는 원을 구현하였다.

  ```java
  public void paint(Graphics g) {
  		radius -= Main.NOTE_SPEED1; // 지름 계속 줄이기
  		g.setColor(color);
  		g.fillOval(centerX - radius / 2, centerY - radius / 2, radius, radius);
  		setNew(false);
  }
  ```

  ```java
  public int nowRadius; // 줄어드는 지름
  public String nowNoteType; // 지금의 노트
  	
  public void noteCircles() { // 노트 그리기
  		for(int i = 0; i < beats.length; ++i) {
  			final int ii = i;
  			final NoteBorder border = new NoteBorder(beats[ii].getTime(),beats[ii].getNoteType());
  			NoteBorder.setNew(true);
  			border.setVisible(false);
  			borderList.add(border);
  		
  			// 노트 생성 시 딜레이 넣는 일회성 타이머
  			Timer timer = new Timer(beats[ii].getTime(), new ActionListener() {
  				@Override
  				public void actionPerformed(ActionEvent e) {
  					border.setVisible(true);
  					
  					// 노트 사이즈 줄이는 반복 타이머
  					Timer t = new Timer(Main.NOTE_SPEED2, new ActionListener() { 
  						@Override
  						public void actionPerformed(ActionEvent e1) {
  							if(border.getRadius() < 0) {
  								((Timer)e1.getSource()).stop();
  								return;
  							}
  							border.resizeCircle(); // repaint()
  							nowRadius = border.getRadius();
  							nowNoteType = beats[ii].getNoteType();
  						}
  					});
  					t.start(); // 반복
  				}
  			} );
  			timer.setRepeats(false); // 한번만 동작
  			timer.start();
  		}
  }
  ```

  

- 관계형 데이터베이스를 활용해 순위를 조회하고 기록을 갱신한다.

  ```java
  private String getMaxScore() throws SQLException { // 순위 조회
  		String sql = "SELECT regdate, name, score FROM scorelist ORDER BY score DESC";
  		ps = conn.prepareStatement(sql);
  		rs = ps.executeQuery();
  		String message = "";
  		while(rs.next()) {
  			message += rs.getString(1) 
  					+ "&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;" 
  					+ "&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;" 
  					+ rs.getString(2) 
  					+ "&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"
  					+ "&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;" 
  					+ rs.getInt(3) + "<br>"; 
  		}
  		return "<html><body>" + message + "<body><html>";
  }
  ```

  ```java
  private void setMaxScore() throws SQLException { // 기록 갱신
  		String sql = "INSERT INTO scorelist(name, score) VALUES(?, ?)";
  		
  		String name = setID.getText();
  		int score = Game.score;
  		
  		ps = conn.prepareStatement(sql);
  		ps.setString(1, name);
  		ps.setInt(2, score);
  		ps.execute();
  }
  ```

  



## 힘들었던 점

##### 1. 원을 구현하는 문제

BufferedImage를 이용해 원 모양의 이미지를 줄이는 방법도 있었다. 하지만 PhotoShop 프로그램이 없어서 반투명한 원의 이미지를 그리는 데에 어려움도 있었을 뿐더러, 좀 더 자연스러운 구현을 위해 Graphics를 쓰고 싶었다. Graphics를 처음 사용하기 때문에 우선은 paint()에 대한 이해가 필요했으며 Frame 혹은 Panel에 그려내는 데에 충돌이 생겨 보이지 않는 문제가 있었다. 그래서 여러 swing 객체들을 한 화면에서 원근감있게 표현할 수 있는 JLayerPane이라는 클래스를 이용하여 해결할 수 있었다.

그리고 fillOval()라는 메서드는 원을 그릴 때 센터좌표를 중심으로 그리지 않고 사각형을 그리듯이 왼쪽 위의 좌표에서 원의 너비 즉 지름만큼의 사각형에 맞는 원을 그린다. 줄어드는 Note는 항상 그 중심이 같아야하므로 이를 해결하기 위해 변하는 지름에 맞게 x, y 좌표도 실시간으로 변하도록 만들어 주었다.



##### 2. 원이 생성되는 시점과 줄어드는 속도를 설정하는 문제

paint()는 해당 클래스가 시작될 때 자동으로 제일 먼저 실행된다. 그래서 실행이 되기 전 delay를 주기 위해 swing.Timer 클래스가 한번 실행되도록 했고, 그 내부에 paint()와 repaint()를 지름이 마이너스가 될 때까지 반복하는 Timer 클래스로 한번 더 사용했다.



## 앞으로 개선할 것들

##### 1. 곡 종류 및 난이도 추가

본 프로그램은 한 곡만 이용해서 코딩했다. 추후에 곡을 추가하고 각 곡의 난이도까지 추가하여 사용자의 선택의 폭을 넓힐 것이다.



##### 2. 콤보 수와 점수, 노트 판정에 시각적 효과 추가

Label을 사용하여 매 Key 입력에 콤보 수, 점수, 등의 게임적 요소가 변하게 구현했지만, 포토샵을 활용한 Image를 추가하여 좀 더 타격감 넘치는 프로그램을 만들 수 있다.



##### 3. 일시정지 및 계속하기 기능 추가

Button을 누르면 Thread가 일시정지 되었다가, 한 번 더 누르면 멈췄던 부분부터 다시 실행되는 기능을 추가하고 싶었으나, 원 형태의 Note를 swing.Timer로 생성, 축소하는 메서드는 일시정지하는 것이 불가능했다. 그 방법을 찾아 개선할 것이다.



##### 4. 동시 입력을 위한 KeyListener 수정

리듬게임에는 분명 두 개 혹은 두 개 이상의 노트를 동시에 누르는 경우도 있다. 하지만 일반적인 KeyListener는 이러한 동시 입력을 제대로 받지 못해서 HashMap을 사용하여 KeyPressed 때 추가, KeyReleased 때 삭제 하는 등의 방법으로 노트 흐름과 난이도를 다양하게 바꿀 수 있다. 



##### 5. 최고 점수 기록을 위한 이름 입력 시 문자 길이 지정

Game Over가 되면 사용자의 점수와 입력한 이름을 DB에 저장하고 그 데이터 중 점수의 내림차순으로 10개의 데이터를 보여주는 점수판을 만들었다. 하지만 이름을 입력할 때 별다른 제약을 두지 않아 그 길이에 따라 뒤에 적히는 점수가 밀리는 현상이 발생했다. 제약사항을 추가하여 조금 더 정렬된 점수판을 만들 것이다.



## 시연영상 및 Javadoc 링크
