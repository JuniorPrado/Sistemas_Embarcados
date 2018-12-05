```C

#include<stdio.h>
#include<unistd.h>
#include<math.h>
#include<opencv/cv.h>
#include<opencv/highgui.h>
#include<opencv2/objdetect/objdetect.hpp>
#include<opencv2/highgui/highgui.hpp>
#include<opencv2/imgproc/imgproc.hpp>
#include<vector>
#include <wiringPi.h>
#include <pthread.h>

#define	BUZZER	0
#define	LED	2
#define MOTOR	3

using namespace std;
using namespace cv;


void *beep (void* arg)
{
  for (int i = 0; i < 10;i++)
  {
    digitalWrite (BUZZER, HIGH) ;	// Liga
    
    delay (100) ;		// 
    
    digitalWrite (BUZZER, LOW) ;	// Desliga
    delay (100) ;
  }

   //delay (100) ;   // 
    digitalWrite (BUZZER, LOW) ; // Desliga
    //delay (100) ;
 return 0;
}

void *vibra(void* arg)
{
  for (int i = 0; i < 10;i++)
  {
    digitalWrite (MOTOR, HIGH) ;	// Liga
    
    delay (100) ;		// 
    digitalWrite (MOTOR, LOW) ;	// Desliga
    delay (100) ;
  }
   digitalWrite (MOTOR, LOW) ;  // Desliga
    //delay (100) ;   // 
    digitalWrite (BUZZER, LOW) ; // Desliga
    //delay (100) ;
 
    return 0;
}


void *pisca(void* arg)
{
  for (int i = 0; i < 1; i++)
  {
    digitalWrite (LED, HIGH) ;	// Liga
    
    delay (100) ;		// 
    digitalWrite (LED, LOW) ;	// Desliga
    delay (100) ;
  }
   digitalWrite (LED, LOW) ;  // Desliga
    //delay (100) ;   // 
    digitalWrite (LED, LOW) ; // Desliga
    //delay (100) ;
 
    return 0;
}

int main(){
    int t[] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0};  
    int i = 0;          
    Mat img,img_cinza;
        CascadeClassifier face_cascade, eye_cascade;
    pthread_t a_thread;
    pthread_t b_thread;
  	pthread_t c_thread;
  	
  	wiringPiSetup () ;
  	pinMode (BUZZER, OUTPUT) ;
  	pinMode (MOTOR, OUTPUT) ;
    pinMode (LED, OUTPUT) ;
    
    if(!face_cascade.load("/home/pi/Desktop/haarcascades/haarcascade_frontalface_alt.xml")) {
        printf("Error loading cascade file for face");
        return 1;
    }
    if(!eye_cascade.load("/home/pi/Desktop/haarcascades/haarcascade_eye.xml")) {
        printf("Error loading cascade file for eye");
        return 1;
    }
    vector<Rect> eyes, faces;

    VideoCapture cap(0);
    Point centere,centerf;
	pthread_create(&a_thread, NULL, beep, NULL);
	pthread_create(&b_thread, NULL, vibra, NULL);
	pthread_create(&c_thread, NULL, pisca, NULL);
				
    while(true){
        cap.read(img);
        

        /*
		Tratamento da imagem capturada passando-a para escala de cinza e equalizando seus tons
        */
        cvtColor( img, img_cinza, CV_BGR2GRAY );
        equalizeHist( img_cinza, img_cinza);
        /*
		Aplicaçao da haarscascade para detectar o rosto
        */
        face_cascade.detectMultiScale( img_cinza, faces, 1.1, 2, 0|CV_HAAR_SCALE_IMAGE, Size(50, 50) );
        
		/*
		Aciona o alerta do LED caso o rosto não seja detectado
		*/
        if(faces.size()==0)
            {
	 			pthread_create(&c_thread, NULL, pisca, NULL);

	         	 printf("Face não detectada %d\n", t);
   
        }
			pthread_join(c_thread,NULL);
			pthread_cancel(c_thread);
        

        for(size_t k=0; k<faces.size(); k++){
            int comp_x=faces[k].x;
            int comp_y=faces[k].y;
            /*
            Localiza o centro do rosto
            */
            centerf=Point(faces[k].x+int(faces[k].width*0.5), int(faces[k].y+faces[k].height*0.5));

            /*
			Armazena a imagem somente do rosto 
            */
            Mat imgf(img_cinza, faces[k]);
            /*
			Faz o desenho de uma elipse ao redor do rosto detectado, desnecessario para o produto final
		   ellipse(img, centerf, Size(faces[k].width*0.40,faces[k].height*0.55),0,0,360,Scalar(0,0,255),2,8,0);
        	*/
            

          
	        /*
			Aplicação da haarcascade para detectar os olhos utlizando a imagem somente do rosto
	        */			
            

            eye_cascade.detectMultiScale( imgf, eyes,  1.1, 3, CV_HAAR_FIND_BIGGEST_OBJECT, Size(30, 30), Size(300,300));		
        	/*
			Aciona os aletas sonoros e vibratorios ao não detectar os olhos em dois frames
        	*/

        	if(eyes.size()==0)
            {
            	printf("incrementa\n");
            	t[i] = 1;

	        }
	    

        /*		   

        Reliza o desenho de uma elipse ao redor dos olhos, desnecessaria para o produto final
                
                for(size_t j=0; j<eyes.size(); j++){
                    centere=Point(eyes[j].x+eyes[j].width*0.5+comp_x, eyes[j].y+eyes[j].height*0.5+comp_y);
                    if(centere.y<centerf.y)
                        ellipse(img, centere, Size(eyes[j].width*0.5,eyes[j].height*0.5),0,0,360,Scalar(232,168,0),2,8,0);
                }*/        
        
        
    	}

    	/*

		Verifica se em dois frames os olhos não foram detectados
    	*/
            
        if (t[i] == 1 && t[i-1] == 1){
        	printf("thread ativada %d %d\n",t[i], t[i-1]);

			pthread_create(&a_thread, NULL, beep, NULL);
			pthread_create(&b_thread, NULL, vibra, NULL);
        
        //t[i] = 0;
        t[i-1] = 0;
        }
        else{

			pthread_join(a_thread,NULL);
			pthread_join(b_thread,NULL);
			pthread_cancel(a_thread);
			pthread_cancel(b_thread);
			i ++;
			printf("valor de t: %d e i : %d\n", t[i], i);
		}

		/*
		Limpa o vetor colocando zeros no lugar de 1 e volta para a posição inicial
		*/
		if (i >= 10)
		{
			for (int i = 10; i >0 ; --i)
			{
				if (t[i] != 0)
				{
					t[i] = 0;
					
				}
			}
			i = 0;

		}
        
        waitKey(250);
        /*
		
        namedWindow("Test",CV_WINDOW_AUTOSIZE);
        

        imshow("Test",img);
        if(waitKey(250)>=0) 
            break;

        */


    }
    destroyAllWindows();
    return 0;
}
```
