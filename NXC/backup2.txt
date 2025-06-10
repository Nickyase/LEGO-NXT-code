#define SENSOR1 IN_1
#define SENSOR2 IN_3
#define MOTOR_LEFT OUT_B
#define MOTOR_RIGHT OUT_C
#define ULTRASONIC IN_4
#define NEAR 15 //em centímetros.

    void avoid_obstacle(){
        RotateMotor(OUT_B, 70, 360);
        RotateMotor(OUT_C, 70, -360);
        Wait(500);
        OnFwd(OUT_BC, 70);
        Wait(2000);
        RotateMotor(OUT_B, 70, -360);
        RotateMotor(OUT_C, 70, 360);
        Wait(500);
        OnFwd(OUT_BC, 70);
        Wait(2000);
        RotateMotor(OUT_B, 70, -360);
        RotateMotor(OUT_C, 70, 360);
        Wait(500);
    }

    void move_forward(int power, int degrees, int time){
        OnFwd(OUT_BC, power, degrees, time);
    }

    // void turn_left(int power, int degrees, int time){       talvez nn use 
    //     OnFwd(OUT_B, power, degrees, time);
    //     OnRev(OUT_C, power, degrees, time);
    // }

    // void turn_right(int power, int degrees, int time){
    //     OnFwd(OUT_C, power, degrees, time);
    //     OnRev(OUT_B, power, degrees, time);
    //}

    void move_backward(int power, int degrees, int time){
        OnRev(OUT_BC, power, degrees, time);
    }

    void stop_robot(){
        Off(OUT_BC);
        StopAllTasks(); 
    }


task main() {
    SetSensorColorFull(SENSOR1);                   // Setando os sensores.
    SetSensorColorFull(SENSOR2);
    SetSensorLowspeed(ULTRASONIC);

    while (true) {
        int color1 = Sensor(SENSOR1);
        int color2 = Sensor(SENSOR2);

        ClearScreen();                             // Limpa o LCD.

        NumOut(0, LCD_LINE1, color1);              // Mostra no LCD o valor numérico
        NumOut(0, LCD_LINE2, color2);              // de cada cor (0-6).

        if (SensorUS(IN_4) > NEAR) {               // Se for maior que 15cm;
        
        // Ajuste os valores conforme necessário
        if (color1 == 5 && color2 == 5){           // 2x Vermelho;
            stop_robot();
        } 

        else if (color1 == 6 && color2 == 6){      // 2x Branco;
            move_forward(50);
        } 

        else if (color1 == 1 && color2 == 1){      // 2x Preto;
            move_forward(50);
        } 

        else if (color1 == 3 && color2 == 3){      // 2x Verde;
            move_forward(50);
        } 

        else if (color1 == 6 && color2 == 1){      // Branco / Preto;
            OnFwd(OUT_B, 25);
            OnRev(OUT_C, 25);
        } 

        else if (color1 == 1 && color2 == 6){      // Preto / Branco;
            OnFwd(OUT_C, 25);
            OnRev(OUT_B, 25);
        } 

        else if (color1 == 3 && color2 == 6){      // Verde / Branco;
            OnFwd(OUT_C, 25);
            OnRev(OUT_B, 25);
        } 

        else if (color1 == 6 && color2 == 3){      // Branco / Verde;
            OnFwd(OUT_B, 25);
            OnRev(OUT_C, 25);
        }

        Wait(200);                                 // Pequena pausa para atualização da tela.
    }

    else {                                         // Se for menor que 15cm;
        avoid_obstacle();
    }
  }
}