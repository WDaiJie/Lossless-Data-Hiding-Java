import java.*;
import javax.swing.filechooser.*;   
import java.awt.Image;
import java.awt.Toolkit;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.image.BufferedImage;
import java.awt.image.ColorModel;
import java.awt.image.DataBufferByte;
import java.awt.image.Raster;
import java.awt.image.SampleModel;
import java.awt.image.WritableRaster;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.UnsupportedEncodingException;

import javax.swing.JFileChooser;
import javax.imageio.ImageIO;
import javax.swing.Icon;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JTextField;
public class hwork {
	protected static final String ImagePath = null;
	 static  byte[] img_t=new byte[257];
	 static   byte[] img_arr;
	 static String char_arr="";
	   static int max,x;
	   static   BufferedImage bufferedImage = null;
	   static String[] s_carr;
	  static String s_label;
	public static void main(String args[]) throws IOException
	 {		 		  
		  JFrame Fame=new JFrame("期末作業(藏文字無失真還原)");
		  Fame.setBounds(200,200,1500,1500);
		  Fame.setLayout(null);		  
		  JFileChooser file = new JFileChooser("選圖片"); 
		  file.setBounds(40,200,100,50);
		  file.setLayout(null);
		  Fame.add(file);		  
		  JButton Button_sec=new JButton("輸出圖片");
		  Button_sec.setBounds(40,300,100,40);
	      Fame.add(Button_sec);	      
	      JTextField input_label=new JTextField("輸入文字數");
	      input_label.setBounds(40,100,120,80);
	      Fame.add(input_label);	     	      
	      JButton b_img = new JButton("輸入圖片");
	      b_img.setBounds(300,300,100,40);
	      Fame.add(b_img);	          
	      JLabel label_img = new JLabel("圖片");
	      label_img.setBounds(300,20,670,250);
	      Fame.add(label_img);		      
	      JLabel label_udimg = new JLabel("藏文字圖片");
	      label_udimg.setBounds(40,380,670,250);
	      Fame.add(label_udimg);	 	      
	      JButton Button_tex=new JButton("解出文字");
		  Button_tex.setBounds(680,380,100,40);
	      Fame.add(Button_tex);	      
	      JLabel output_label=new JLabel("輸出文字");
	      output_label.setBounds(680,450,120,80);
	      Fame.add(output_label);	      
	      JButton Button_oimg=new JButton("解出原圖");
	      Button_oimg.setBounds(880,380,100,40);
	      Fame.add(Button_oimg);	      
	      JLabel label_odimg=new JLabel("輸出原圖");
	      label_odimg.setBounds(880,450,670,250);
	      Fame.add(label_odimg);	      	      
	      Fame.setVisible(true);
	      Fame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	      Button_sec.addActionListener( new ActionListener()
     {
   	 public void actionPerformed(ActionEvent  a)
   	 { 		   		 
   	 }
      });
    Fame.addWindowListener(new WindowAdapter()
		 {
	       public void windowClosing(WindowEvent e1)
	       {	    
	    	   System.exit(0);
	       }      
		 });   	
    b_img.addActionListener(new ActionListener() {
		@Override
		public void actionPerformed(ActionEvent e) {
			  
		      JFileChooser fc = new JFileChooser(); 	
		      file.setCurrentDirectory(new File(System.getProperty("user.home")));
		      fc.setBounds(150,300,100,60);
			  FileNameExtensionFilter filter = new FileNameExtensionFilter("*.Images", "jpg","gif","png");
	          fc.addChoosableFileFilter(filter);
	          int result = fc.showSaveDialog(null);
	          if(result == JFileChooser.APPROVE_OPTION){
	              File selectedFile = fc.getSelectedFile();
	              String path = selectedFile.getAbsolutePath();
	              System.out.println(path);
	              label_img.setIcon(ResizeImage(path)
	            		  );	   
	              BufferedImage bi = null;    
	              File imgPath = new File(path);
	          
				try {
					bufferedImage = ImageIO.read(imgPath);
				} catch (IOException e1) {
					e1.printStackTrace();
				}
	              WritableRaster raster = bufferedImage .getRaster();
	              DataBufferByte data   = (DataBufferByte) raster.getDataBuffer();
	              img_arr=data.getData();	            	
	                  for(int k=0;k<257;k++)
	                  {
	                	  img_t[k]=0;
	                  }
//	                  for(int i=0;i<img_arr.length;i++)
//	                  {
//	                	   System.out.printf("原本:%d=%d,",i,img_arr[i]);
//	                	   System.out.printf("(範圍255)%d=%d,",i,img_arr[i]&0xff);	  
//	                	   System.out.printf("還原:%d=%d,\n",i,(byte)(img_arr[i]&0xff));	 
//	                  }
	                  for(int k=0;k<257;k++)
 	                  {
		                  for(int i=0;i<img_arr.length;i++)
		                  {
			                 
		                	if((img_arr[i]&0xff)==k)
			                { 
			                		   img_t[k]++;
			                 }		 	                  
		                  }
 	                  }
	                   x=0;max=0;
	                  System.out.println("");
	                  for(int k=0;k<img_t.length;k++)
	                  {
	                	  System.out.printf("計算%d=%d, ",k,img_t[k]);
	                  }
	                  for(int j=0;j<img_t.length;j++)
	                  {
	                	 
	                	  if(img_t[j]>x)
                          {
    	                           x=img_t[j];
    	                           max=j;
                          }
	                  }
	                  System.out.printf("最大%d\n",max);
	                for(int j=255;j>0;j--)
	                {                	
	                	if(j>max)
	                	{
	                		img_t[j+1]=img_t[j];
	                	}	                	
	                }	                
	                System.out.println(" ");
	                for(int j=0;j<257;j++)
	                {
	                	   img_t[max+1]=0;
	                	  System.out.printf("計算%d=%d, ",j,img_t[j]);
	                }
	                System.out.printf("\n");
	                for(int i=0;i<img_arr.length;i++)
	                {
	                  if((img_arr[i])>max)
	                  {
	                	  (img_arr[i])++;
	                	  System.out.printf("個開:%d=%d,\n",i,img_arr[i]);
	                  }	                	
	                }    
	                for(int i=0;i<img_arr.length;i++)
	                  {
	                	   System.out.printf("原本:%d=%d",i,img_arr[i]);
//	                	   System.out.printf("(範圍255)%d=%d,",i,img_arr[i]&0xff);	  
//	                	   System.out.printf("還原:%d=%d,\n",i,(byte)(img_arr[i]&0xff));	 
	                  }
	          }  
	          else if(result == JFileChooser.CANCEL_OPTION){
	              System.out.println("No File Select");
	          }	         	        
		}
		private Icon ResizeImage(String path) {
			  ImageIcon MyImage = new ImageIcon(path);
		        Image img = MyImage.getImage();
		        Image newImg = img.getScaledInstance(100,100, Image.SCALE_SMOOTH);
		        ImageIcon image = new ImageIcon(newImg);
		        return image;
		}
    });  
    Button_sec.addActionListener(new ActionListener() {  	
    	public void actionPerformed(ActionEvent e) {
    		 s_label = input_label.getText();
    		System.out.println(s_label);
    		   byte [] s_byte=s_label.getBytes();
    		   for(int i=0;i<s_byte.length;i++)
               {   		
    			   System.out.println((s_byte[i]&0xff)); 
    			   System.out.printf("還原:%d\n",(s_byte[i]));	 
  			       char_arr=char_arr+Integer.toBinaryString(s_byte[i]&0xff);
    			   System.out.println(Integer.toBinaryString(s_byte[i]&0xff));     			    			      	              
               }   		 
    		   System.out.println(char_arr);  
    		   System.out.printf("長度:%d\n",char_arr.length());  
    		   int c=0;
    		   char f;
    		   int j=0;
    		   int sc=0;
    		   while(c<char_arr.length())
    		   {   			    
    			   f=char_arr.charAt(c);    			
    	    	   if((img_arr[j]==max))
    	    	   {  
    	    		   System.out.printf("原來max:%d=%d",j,img_arr[j]); 
    	    		   if((f=='1'))
    	    		   {
    	    			   sc++;
    	    			   img_arr[j]++;
    	    			   System.out.printf("sc:%d\n",sc);
        	    		   System.out.printf("藏max:%d=%d,\n",j,img_arr[j]);    
    	    		   }  
    	    		   else  if((f=='0'))
    	    		   {
    	    			   System.out.printf("\n");
    	    		   }
    	    		   c++;  
    	    	   }  
    	    	   j++;    	    	    	    	             	
               }
    		   for(int g=0;g<img_arr.length;g++)
               {
    			   System.out.printf("藏圖片:%d=%d,\n",g,img_arr[g]);    
               }   		
    		   DataBufferByte bais = new DataBufferByte(img_arr, img_arr.length);
    		   ColorModel colorModel = bufferedImage.getColorModel();
    		   SampleModel s1=bufferedImage.getRaster().getSampleModel();
    		   Raster raster = Raster.createWritableRaster(s1, bais, null);
    		   WritableRaster writableRaster = raster.createCompatibleWritableRaster();
    		   writableRaster.setDataElements(0, 0, raster);
    		   BufferedImage img_u = new BufferedImage(colorModel, writableRaster, colorModel.isAlphaPremultiplied(), null);									
					   try {
							ImageIO.write(img_u,"jpg", new File("c://img/hide.jpg"));
						} catch (IOException e1) {
							e1.printStackTrace();
						}
			    		   System.out.println("success");
			    		   label_udimg.setIcon(ResizeImage("c://img/hide.jpg"));					
               System.out.println("image created");   		   	 
    		   System.out.println(" ");
    		   String t = new  String(s_byte );
    	       System.out.println(t);    	          	         	       
    	}    
    	private Icon ResizeImage(String path) {
			  ImageIcon MyImage = new ImageIcon(path);
		        Image img = MyImage.getImage();
		        Image newImg = img.getScaledInstance(100,100, Image.SCALE_SMOOTH);
		        ImageIcon image = new ImageIcon(newImg);
		        return image;
		}   	       
    });
    Button_tex.addActionListener(new ActionListener() {  	
    	public void actionPerformed(ActionEvent e) {
    		String s_char="";
    		int c=0;
    		  for(int g=0;g<=img_arr.length;g++)
              {
    		  
   			   System.out.printf("解藏圖片:%d=%d,\n",g,img_arr[g]);  
   			   if((img_arr[g]==max))
   			   {
   				   s_char=s_char+'0';
   				   c++;
   			   }
   			   else   if((img_arr[g]==max+1))
   			   {
   				 s_char=s_char+'1';
   				 c++;
   			   }
   			  if(c>=char_arr.length())
  		       {
  			     break;
  		      }  			   
              }
    		  System.out.printf("解出文字:%s\n",s_char);  
    		 int sca=0;
    	       byte[] s_txt=new byte[s_label.length()];
             for(int y=0;y<s_char.length();y=y+7)
             {
            	System.out.printf("切出文字:%d\n", (Integer.parseInt((String)s_char.subSequence(y,y+7),2)));  
                s_txt[sca]=(byte)(Integer.parseInt((String)s_char.subSequence(y,y+7),2));
                sca++;            	
             }
             System.out.println(" ");
  		     String t = new  String(s_txt);
  	         System.out.println(t);    	         
  	       output_label.setText(t);
    	} 
    });
    Button_oimg.addActionListener(new ActionListener() {  	
    	public void actionPerformed(ActionEvent e) {
    		int c=0;
    		  for(int g=0;g<=img_arr.length;g++)
              {
    			  System.out.printf("解藏圖片:%d=%d,\n",g,img_arr[g]);  
    			   if((img_arr[g]==max))
       			   {      				   
       				   c++;
       			   }
       			   else   if((img_arr[g]==max+1))
       			   {
       				img_arr[g]--;
       				 c++;
       			   }
       			  if(c>=char_arr.length())
      		       {
      			     break;
      		      }
              } 
    		  for(int i=0;i<img_arr.length;i++)
    	         {
    	           if((img_arr[i])>max)
    	           {
    	         	  (img_arr[i])--;   	         	 
    	           }              	
    	         }
    		  for(int i=0;i<img_arr.length;i++)
 	         {
    		  System.out.printf("還原:%d=%d",i,img_arr[i]);
 	         }    	    
    		   DataBufferByte bais = new DataBufferByte(img_arr, img_arr.length);
    		   ColorModel colorModel = bufferedImage.getColorModel();
    		   SampleModel s=bufferedImage.getRaster().getSampleModel();
    		   Raster raster = Raster.createWritableRaster(s, bais, null);
    		   WritableRaster writableRaster = raster.createCompatibleWritableRaster();
    		   writableRaster.setDataElements(0, 0, raster);
    		   BufferedImage img_u = new BufferedImage(colorModel, writableRaster, colorModel.isAlphaPremultiplied(), null);
    		   try {
				ImageIO.write(img_u,"jpg", new File("c://img/after.jpg"));
			} catch (IOException e1) {
				e1.printStackTrace();
			}
    		   System.out.println("success");
    					label_odimg.setIcon(ResizeImage("c://img/after.jpg"));	  
    	}   	
    	private Icon ResizeImage(String path) {
			  ImageIcon MyImage = new ImageIcon(path);
		        Image img = MyImage.getImage();
		        Image newImg = img.getScaledInstance(100,100, Image.SCALE_SMOOTH);
		        ImageIcon image = new ImageIcon(newImg);
		        return image;
		}   	
    });    	    
	 }	
}
