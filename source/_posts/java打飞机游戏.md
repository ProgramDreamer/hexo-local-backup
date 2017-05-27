---
title: java打飞机游戏
date: 2017-05-26 22:49:46
tags: java

---


### 项目图片示例： ###

![项目示例](/img/06.png)

## 项目结构目录 ##

<!--more-->
- src...
	- main
		- java
			- config
			- entity
			- factory
			- listener
			- ui
			- util
		- resources
			- images
			- sound
	- test
		- java
		- resources


## 关于本项目说明 ##

- 使用IntelliJIDEA编译运行
- 源码放在java文件夹下
- 图片，声音资源放在resources文件夹下
- main.java运行

### 图片加载 ###
    public class ImageLoader {

        private BufferedImage sourceImg;

        public ImageLoader(String imagePath) throws IOException {
	    sourceImg = ImageIO.read(new File(imagePath));
    }

    	public Image getImage(int posX, int posY, int width, int height) {
		BufferedImage targetImg = this.sourceImg.getSubimage(posX, posY, width, height);
		Image img = new ImageIcon(targetImg).getImage();
		return img;
    	}
	}

### 声音加载 ###

	public class SoundPlayer {
    private Clip clip;

    public SoundPlayer(String filePath) throws LineUnavailableException, UnsupportedAudioFileException, IOException {
	File file = new File(filePath);
	AudioInputStream audioInputStream = AudioSystem.getAudioInputStream(file);
	clip = AudioSystem.getClip();
	clip.open(audioInputStream);
    }

    public void play() {
	clip.setFramePosition(0);
	clip.start();
    }

    public void loop() {
	clip.loop(Clip.LOOP_CONTINUOUSLY);
    }

    public void stop() {
	clip.stop();
    }

	}

### 文件处理 ###

	public class FileUtil {
    public static String readFileToString(String filePath) throws IOException {
	StringBuilder sb = new StringBuilder();
	File file = new File(filePath);
	BufferedReader br = new BufferedReader(new FileReader(file));
	String line = null;
	while ((line = br.readLine()) != null) {
	    sb.append(new String(line.getBytes(),"utf-8")).append("\r\n");
	}
	br.close();
	return sb.toString();
    }

    public static void writeScore(List<Score> scoreList, String filePath) throws FileNotFoundException, IOException {
	ObjectOutputStream objOutputStream = new ObjectOutputStream(new FileOutputStream(filePath));
	for (Score score : scoreList) {
	    objOutputStream.writeObject(score);
	}
	objOutputStream.writeObject(null);
	objOutputStream.flush();
	objOutputStream.close();
    }

    public static List<Score> readScore(String filePath) throws FileNotFoundException, IOException,
	    ClassNotFoundException {
	List<Score> scoreList = new ArrayList<Score>();
	ObjectInputStream objInputStream = new ObjectInputStream(new FileInputStream(filePath));
	Object obj = null;
	while ((obj = objInputStream.readObject()) != null) {
	    scoreList.add((Score) obj);
	}
	objInputStream.close();
	return scoreList;
    }
}

>github源码

源码请参见[github：https://github.com/ProgramDreamer](https://github.com/ProgramDreamer/Plane)

