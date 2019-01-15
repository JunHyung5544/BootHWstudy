## 7segment test program code 1

    #define __DELAY_BACKWARD_COMPATIBLE__    // _delay_ms()함수에 변수가 인수로 들어갈시 발생하는 에러를 해결해준 코드:일종의 Flag역할
    #include <avr/io.h>
    #include <util/delay.h>
    unsigned char FND_SEG[10] = {0x3F, 0x06, 0x5B, 0x4F,   0x66, 0x6D, 0x7C, 0x07, 0x7F, 0x67};
    void init_system(void)
    {	
        DDRA = 0xFF; 
	    DDRE = 0x0C;    //LED_CTRL, LED_DATA 각각 DDRE레지스터의 2,3번비트에 해당 출력모드로 설정필요
	    PORTE = 0x04;
	    PORTA = 0x01;
    }

    int main(void)
    {   
        init_system();
        int j;  
        int k = 70;
	    for(j=0;j<4;j++)
	    {   
            PORTE = 0x04;   //LED_CTRL(LE signal) for U7 *U7: 어떤 7segment를 점등할지 선택하는 출력신호를 저장
		PORTA = 0x01 << j; // j:0~3 
		
		PORTE = 0x08;   // LED_DATA(LE signal) for U6 *U6: 특정 7segment에 어떤 숫자값을 출력해서 보여줄지를 저장
		PORTA = FND_SEG[j]; // PORTA를 통한 숫자값 출력
		_delay_ms(1000);
	    }
	    while(1)
	        {
		    for(j=0;j<4;j++)
		    {   
			PORTE = 0x04;
			PORTA = 0X01 << j;
			PORTE = 0x08;
			PORTA = FND_SEG[j];
			_delay_ms(k);
		    }
	            if(k>=2) //최종적인 delaytime for shift는  1ms
		    k = k-1;
	        }
	return 0;
    }
