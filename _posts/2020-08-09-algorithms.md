---
title: "Algorithms and Data Structures Artifact"
date: 2020-08-09
tags: [algorithms, data structures, computer science, javascript, html]
header:
  image: "/images/js.jpg"
excerpt: "Algorithms, Data Structures, Computer Science, JavaScript, HTML"
---

Greetings!

This project is my Algorithms and Data Structures Artifact for CS 499. This is a project focusing on the use of JavaScript and HTML to create a slideshow for a fictitious travel agency. This project was originally created by me and was a slideshow for the top 5 vactaion destinations.
The orginal program would open a window on the user's desktop to present them with the slideshow that allowed them to use a previous and next button to navigate through the slideshow. For my CS 499 enhancement, I changed the slideshow to show the top 10 destinations, and gave the slideshow a new button
that would take the user to the website of the vacation desination slide they were on. The new button opened the website of the respective destination in a new window on the users desktop.

### Algorithms and Data Structures Artifact:
```js
import java.awt.BorderLayout;
import java.awt.CardLayout;
import java.awt.EventQueue;
import java.awt.FlowLayout;
import java.awt.HeadlessException;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import java.awt.Color;
import java.awt.Desktop;
import java.net.URI; // (CS 499 Artifact 2 Addition)
import java.net.URISyntaxException;
import java.net.URL;

public class SlideShow extends JFrame {

	//Declare Variables
	private JPanel slidePane;
	private JPanel textPane;
	private JPanel buttonPane;
	private CardLayout card;
	private CardLayout cardText;
	private JButton btnPrev;
	private JButton btnNext;
	private JButton btnLink; // (CS 499 Artifact 2 Addition)
	private JLabel destinationSlide;
	private JLabel destinationTextArea;
	
	// URL Variables (CS 499 Artifact 2 Addition)
	String destination1 = "https://www.rythmia.com";
	String destination2 = "https://www.inanitah.com";
	String destination3 = "https://www.sivanandabahamas.org";
	String destination4 = "https://www.skyterrawellness.com";
	String destination5 = "https://www.postranchinn.com";
	String destination6 = "https://www.rimrockresort.com";
	String destination7 = "https://www.peppers.com/au/cradle-mountain-lodge.com";
	String destination8 = "https://www.clubmed.ca/r/Sahoro-Hokkaido/w";
	String destination9 = "https://www.sandals.com/grande-st-lucian/";
	String destination10 = "https://www.hyatt.com/en-US/hotel/colorado/grand-hyatt-vail/egegh";

	/**
	 * Create the application.
	 */
	public SlideShow() throws HeadlessException {
		initComponent();
	}

	/**
	 * Initialize the contents of the frame.
	 */
	private void initComponent() {
		//Initialize variables to empty objects
		card = new CardLayout();
		cardText = new CardLayout();
		slidePane = new JPanel();
		textPane = new JPanel();
		textPane.setBackground(Color.BLUE);
		textPane.setBounds(5, 470, 790, 50);
		textPane.setVisible(true);
		buttonPane = new JPanel();
		btnPrev = new JButton();
		btnNext = new JButton();
		btnLink = new JButton(); // (CS 499 Artifact 2 Addition)
		destinationSlide = new JLabel();
		destinationTextArea = new JLabel();

		//Setup frame attributes
		setSize(800, 600);
		setLocationRelativeTo(null);
		setTitle("Top 10 Destinations SlideShow");
		getContentPane().setLayout(new BorderLayout(10, 50));
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

		//Setting the layouts for the panels
		slidePane.setLayout(card);
		textPane.setLayout(cardText);
		
		//logic to add each of the slides and text
		for (int i = 1; i <= 10; i++) {
			destinationSlide = new JLabel();
			destinationTextArea = new JLabel();
			destinationSlide.setText(getResizeIcon(i));
			destinationTextArea.setText(getTextDescription(i));
			slidePane.add(destinationSlide, "card" + i);
			textPane.add(destinationTextArea, "cardText" + i);
		}

		getContentPane().add(slidePane, BorderLayout.CENTER);
		getContentPane().add(textPane, BorderLayout.SOUTH);

		buttonPane.setLayout(new FlowLayout(FlowLayout.CENTER, 20, 10));

		btnPrev.setText("Previous");
		btnPrev.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				goPrevious();
			}
		});
		buttonPane.add(btnPrev);

		btnNext.setText("Next");
		btnNext.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				goNext();
			}
		});
		buttonPane.add(btnNext);
		
		/**
		* btnLink is CS 499 Artifact 2 Addition
		*/
		btnLink.setText("Website");
		btnLink.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				goLink();
			}
		});
		buttonPane.add(btnLink);

		getContentPane().add(buttonPane, BorderLayout.SOUTH);
	}

	/**
	 * Previous Button Functionality
	 */
	private void goPrevious() {
		card.previous(slidePane);
		cardText.previous(textPane);
	}
	
	/**
	 * Next Button Functionality
	 */
	private void goNext() {
		card.next(slidePane);
		cardText.next(textPane);
	}
	
	/**
	 * Link Button Functionality (CS 499 Artifact 2 Addition)
	 * FIXME: This method currently only opens URL for destination1 in a new window.
	 */
	private void goLink() {
		for (int i = 1; i <= 10;) {
			if (i==1) {
				try {
					Desktop.getDesktop().browse(new URL(destination1).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==2) {
				try {
					Desktop.getDesktop().browse(new URL(destination2).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==3) {
				try {
					Desktop.getDesktop().browse(new URL(destination3).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==4) {
				try {
					Desktop.getDesktop().browse(new URL(destination4).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==5) {
				try {
					Desktop.getDesktop().browse(new URL(destination5).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==6) {
				try {
					Desktop.getDesktop().browse(new URL(destination6).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==7) {
				try {
					Desktop.getDesktop().browse(new URL(destination7).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==8) {
				try {
					Desktop.getDesktop().browse(new URL(destination8).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==9) {
				try {
					Desktop.getDesktop().browse(new URL(destination9).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}else if (i==10) {
				try {
					Desktop.getDesktop().browse(new URL(destination10).toURI());
				}catch (Exception e) {
					e.printStackTrace();
				}
			}
			return;
		}
	}	

	/**
	 * Method to get the images (i == 6 through i == 10 is CS 499 Artifact 2 Addition)
	 */
	private String getResizeIcon(int i) {
		String image = ""; 
		if (i==1){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/Rythmia.jpg") + "'</body></html>";
		} else if (i==2){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/Inanitah.jpg") + "'</body></html>";
		} else if (i==3){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/Sivananda.jpg") + "'</body></html>";
		} else if (i==4){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/Skyterra.jpg") + "'</body></html>";
		} else if (i==5){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/Post.jpg") + "'</body></html>";
		} else if (i==6){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/rimrock.jpg") + "'</body></html>";
		} else if (i==7){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/peppers.jpg") + "'</body></html>";
		} else if (i==8){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/sahoro.jpg") + "'</body></html>";
		} else if (i==9){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/stlucian.jpg") + "'</body></html>";
		} else if (i==10){
			image = "<html><body><img width= '800' height='500' src='" + getClass().getResource("/resources/grandhyatt.jpg") + "'</body></html>";
		}
		return image;
	}
	
	/**
	 * NEW COMMENTS ADDED 2/7/2019
	 * Method to get the text values
	 * Added a short description under each destination main title. To put the description under the title I used the <br> tag.
	 * Re-formated the HTML text values. Changed the font to white so it would be more visible. To do this I added the color element to the first <font> tag, and then added two more <font> tags. One for the description and one for the photo link.
	 * Updated the text size for each destination title text. I kept the number 1 destination title the same size, but increased the font size for the other 4 destinations so that the title appeared larger than the description.
	 * Included photo credit with a hyperlink that takes the user to the page where the photo was found. For this part I used both an <i> tag to style the text in italics, and an <a> tag to create the hyper link around the photo comment. I also styled the anchor tag so that the link would open in a new tab.
	 * The text lines are pretty long and in the future I need to clean those up so I put the styling in a different section, and this text can get those style elements through the script. This should make the code cleaner.
	 * (i == 6 through i == 10 is CS 499 Artifact 2 Addition)
	 */
	private String getTextDescription(int i) {
		String text = ""; 
		if (i==1){
			text = "<html><body><font size='5', color= WHITE>#1 Rythmia Life Advancement Center - Pinilla, Costa Rica.</font> <br><font color = WHITE>Awaken to miracles in this tropical destination.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.rythmialifeadvancement.com/gallery' target='_blank'>Photo Courtesy of Rythmia</a></i></font></body></html>";
		} else if (i==2){
			text = "<html><body><font size='4', color= WHITE>#2 Inanitah Wellness Retreat - Volcan Maderas, Nicaragua.</font> <br><font color = WHITE>Rejuvenate your life among the water and volcanic backdrops.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.inanitah.com/' target='_blank'>Photo Courtesy of Inanitah</a></i></font></body></html>";
		} else if (i==3){
			text = "<html><body><font size='4', color= WHITE>#3 Sivananda Ashram Yoga Retreat - Paradise Island, Bahamas.</font> <br><font color = WHITE>Simplify and reset yourself through the power of yoga.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.sivanandabahamas.org/' target='_blank'>Photo Courtesy of Sivananda</a></i></font></body></html>";
		} else if (i==4){
			text = "<html><body><font size='4', color= WHITE>#4 Skyterra Wellness Retreat - Lake Toxaway, North Carolina.</font> <br><font color = WHITE>Immerse yourself in the beautiful Blue Ridge Mountains.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.skyterrawellness.com/explore-skyterra/location/' target='_blank'>Photo Courtesy of Skyterra</a></i></font></body></html>";
		} else if (i==5){
			text = "<html><body><font size='4', color= WHITE>#5 Post Ranch Inn - Big Sur, California.</font> <br><font color = WHITE>Experience the natural wonder of the Pacific Ocean.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.postranchinn.com/' target='_blank'>Photo Courtesy of Post Ranch Inn</a></i></font></body></html>";
		} else if (i==6){
			text = "<html><body><font size='4', color= WHITE>#6 The Rimrock Resort - Banff, Alberta, Canada.</font> <br><font color = WHITE>Majestic Canadian mountain views.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.rimrockresort.com/' target='_blank'>Photo Courtesy of The Rimrock Resort</a></i></font></body></html>";
		} else if (i==7){
			text = "<html><body><font size='4', color= WHITE>#7 Peppers Cradle Mountain Lodge - Cradle Mountain, TAS, Austrilia.</font> <br><font color = WHITE>Enjoy a relaxing vacation down under.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.peppers.com.au/cradle-mountain-lodge/' target='_blank'>Photo Courtesy of Peppers Cradle Mountain Lodge</a></i></font></body></html>";	
		} else if (i==8){
			text = "<html><body><font size='4', color= WHITE>#8 Club Med - Sahoro-Hokkaido, Japan.</font> <br><font color = WHITE>Excellent winter get away with great skiing.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.clubmed.ca/r/Sahoro-Hokkaido/w' target='_blank'>Photo Courtesy of Club Med Sahoro-Hokkaido</a></i></font></body></html>";
		} else if (i==9){
			text = "<html><body><font size='4', color= WHITE>#9 Sandals Grande St. Lucian - Gros-Islet, Saint Lucia.</font> <br><font color = WHITE>Pitcure perfect white sandy beaches.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.sandals.com/grande-st-lucian/' target='_blank'>Photo Courtesy of Sandals Grande St. Lucian</a></i></font></body></html>";
		} else if (i==10){
			text = "<html><body><font size='4', color= WHITE>#10 Grand Hyatt Vail - Vail, Colorado.</font> <br><font color = WHITE>Amazing ski-in/ski-out luxury resort.</font><&nbsp><&nbsp><font color = WHITE><i><a href='https://www.hyatt.com/en-US/hotel/colorado/grand-hyatt-vail/egegh' target='_blank'>Photo Courtesy of Grand Hyatt Vail</a></i></font></body></html>";	
		}
		return text;
	}
	
	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {

			@Override
			public void run() {
				SlideShow ss = new SlideShow();
				ss.setVisible(true);
			}
		});
	}
}

```
