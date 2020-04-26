java를 활용하여 자판기 알고리즘을 제작하였습니다.

<소스코드>
```java
package 자판기;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class VendingMachine extends JFrame{
  private MoneyHole mh = new MoneyHole();
   private FoodButtons fm = new FoodButtons();
   private Chages ch = new Chages();
   
   static int UserMoneySum = 0;
   private int [] foodPrice = {4500,4500,4500,9500};
   private int MoneySum = 0;
   private int [] cell = {0,0,0,0};

   static JLabel sumLabel = new JLabel("      투입금액 " + UserMoneySum + "원" );
   
   static ImageIcon [] images = { new ImageIcon("images\\1.png"), new ImageIcon("images\\2.png"),new ImageIcon("images\\3.png")
         ,new ImageIcon("images\\4.png")
   };
   static ImageIcon [] imagesF = { new ImageIcon("images\\1.png"), new ImageIcon("images\\2.png"),new ImageIcon("images\\3.png")
         ,new ImageIcon("images\\4.png")
   };
   
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
   public VendingMachine() 
   {
      setTitle("자판기");
      setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      Container c = getContentPane();
      c.setLayout(null);
      
      createMenu();
      
      setSize(800,450);
      
 
      mh.setBounds(0, 1, getWidth(), 50);
      fm.setBounds(0, 50, getWidth(), 300);
      ch.setBounds(0, 350, getWidth(), 50);
     
   
      c.add(mh);
      c.add(fm);
      c.add(ch);
      
      setVisible(true);
   }
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////   

   private void createMenu() 
   {
      JMenuBar mb = new JMenuBar();
      JMenuItem [] menuItem = new JMenuItem[2];
      String [] itemTitle = {"매출 및 판매현황" , "나가기"};         
      JMenu screenMenu = new JMenu("메뉴");
      
      MenuActionListener listener = new MenuActionListener();
      for(int i=0; i<menuItem.length; i++) 
      {
         menuItem[i] = new JMenuItem(itemTitle[i]);   
         menuItem[i].addActionListener(listener);
         screenMenu.add(menuItem[i]); 
      }
      mb.add(screenMenu); 
      setJMenuBar(mb);
   }
   
   class MenuActionListener implements ActionListener {
       public void actionPerformed(ActionEvent e)
       {
          String cmd = e.getActionCommand();
          switch(cmd)
          {
             case "매출 및 판매현황" :  
                break;
                
             case "나가기" :
                System.exit(0);  
                break;
          }
       }
   }

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
   
   class MoneyHole extends JPanel {
   
      private JButton [] btn = { new JButton("10,000 원"), new JButton("5,000 원")}; 
      
      public MoneyHole() {
         MyMoneyListener listener = new MyMoneyListener();
         for(int i=0; i<2; i++)
         {
            btn[i].setBackground(Color.GREEN);
            btn[i].setFont(new Font("Serif", Font.BOLD, 20));
            add(btn[i]);
            btn[i].addActionListener(listener);
         }
         sumLabel.setFont(new Font("Serif", Font.BOLD, 20));
         add(sumLabel);
         
         setBackground(Color.GRAY);
      }
      
      class MyMoneyListener implements ActionListener {
         
         public void actionPerformed(ActionEvent e)
         {
            JButton b = (JButton)e.getSource();
            if(b == btn[0])
            {
               UserMoneySum += 10000;
            }
            else if(b == btn[1])
            {
               UserMoneySum += 5000;
            }
            sumLabel.setText("      남은금액 " + UserMoneySum + "원");
         }
      }
      
   }
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
   
   class FoodButtons extends JPanel {
      private JButton [] btn = new JButton[4];
      private JLabel [] la = new JLabel[4];
      
      public FoodButtons() 
      {
         setLayout(new GridLayout(2, 2, 5, 5));
         int price = 4500;
         MyMenuListener listener = new MyMenuListener();
         
         for(int i = 0 ; i<4; i++ ) 
         {
            if(i>2) price = 9500;
            
            btn[i] = new JButton(price+"원", images[i]);   
            add(btn[i]);
            btn[i].addActionListener(listener);
         }
         
         setBackground(Color.GREEN);
         }
      class MyMenuListener implements ActionListener{
         
         public void actionPerformed(ActionEvent e)
         {
            
            for(int i = 0 ; i<4; i++ )
            {
               if(UserMoneySum >= foodPrice[i])
               {
                  JButton b = (JButton)e.getSource();
                  if(b == btn[i])
                     {
                   
                        {
                        Foods fd = new Foods();
                        fd.add(new JLabel(imagesF[i]));
                        UserMoneySum -= foodPrice[i]; 
                        MoneySum += foodPrice[i];   
                        cell[i]++; 
                        
                        sumLabel.setText("      남은금액 " + UserMoneySum + "  원");  
                        break;
                        }
                     }      
               }
               else 
               {
                  JOptionPane.showMessageDialog(null, "금액이 부족합니다!", "잔액 부족!", JOptionPane.ERROR_MESSAGE);
                  break;  
               }
            }   
            
         }
      }
}
   
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
   
   class Foods extends JFrame {   
      public Foods() 
      {
        
      }
   }
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
   class Chages extends JPanel {
      private JButton btn = new JButton("잔돈 반환");   
      private JLabel la = new JLabel("");
      public Chages() 
      {   
         btn.addActionListener(new ActionListener() {
            
            public void actionPerformed(ActionEvent e) 
            {
               la.setFont(new Font("Serif", Font.ITALIC, 20));
               
               if(UserMoneySum > 0)  
               {
                  la.setText(UserMoneySum + "잔돈 반환");      
                  UserMoneySum=0;  
                  sumLabel.setText("      남은금액" + UserMoneySum + "  원");
               }
               else 
                  la.setText("돈이 부족합니다");  
            }
         });
         
         btn.setBackground(Color.LIGHT_GRAY);
         setBackground(Color.GRAY);
         add(btn); add(la);
      }
      
      
   }
   
////////////////////////////////////////////////////////  
   public static void main(String[] args) {
      new VendingMachine();

   }

}
```

실행 화면입니다.

타이틀

![자판기](https://blogfiles.pstatic.net/MjAyMDA0MjZfMTkg/MDAxNTg3ODg2NTcxMDQw.oMFyyfjBvrA4QY1p21IqtzYp4sgdJopebE3aLs9YqMYg.DWBVkX7rNgrtAjfP6la4J-7hhmt9zj2gNaHbk3kXsO8g.JPEG.my980127/%EC%B2%AB%ED%99%94%EB%A9%B4.jpg)

금액 투입

![금액 투입](https://blogfiles.pstatic.net/MjAyMDA0MjZfNTYg/MDAxNTg3ODg2NTcwMzcx.k2M6N_Pxjsu7nCXwtYieh_BQwIKl4pNj_V7yJTtG1acg.VHxk5ChQOBPAXx3QlP3fS4uEBSRN-RPhPZOIh0AOe-Yg.JPEG.my980127/%EA%B8%88%EC%95%A1%ED%88%AC%EC%9E%85.jpg?type=w3)

메뉴 선택 후..

잔돈 반환

![잔돈 반환](https://blogfiles.pstatic.net/MjAyMDA0MjZfNTYg/MDAxNTg3ODg2NTcwNzM3.tu6I4IzIECrsdfr10hBFkgfHkTPRcE-TSBPU5SPEgmEg.E8LtB6JJYaHHXEhODrYMmtUHwmqxVBob2vjv2YaGY8wg.JPEG.my980127/%EC%9E%94%EB%8F%88%EB%B0%98%ED%99%98.jpg)
