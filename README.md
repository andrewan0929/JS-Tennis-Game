# JS-Tennis-Game
Udemy tutorial

<html>
    
    <head>
        <title>Tennis Game</title>
    </head>
    
    <canvas id="gameCanvas" width="800"
            height="600"></canvas>
    
    <!--javascript below-->
    <script>
        var canvas;
        var canvasContext;
        var ballX = 50;
        var ballY = 50;
        var ballSpeedX = 10;
        var ballSpeedY = 4;
        
        var player1Score = 0;
        var player2Score = 0;
        const WINNING_SCORE = 5;
        
        
        var paddle1Y = 250;
        var paddle2Y = 250;
        const PADDLE_HEIGHT = 100;
        const PADDLE_THICKNESS = 10;
        
        //code for paddle movement;
        function calculateMousePos(evt) {
            var rect = canvas.getBoundingClientRect();
            var root = document.documentElement;
            var mouseX = evt.clientX - rect.left - root.scrollLeft;
            var mouseY = evt.clientY - rect.top - root.scrollTop;
            return{
                x:mouseX,
                y:mouseY,
            };
        }
        
        
        //background game code;
        window.onload = function() {
            canvas = document.getElementById('gameCanvas');
            canvasContext = canvas.getContext('2d');   
            
            var framesPerSecond = 30;
            setInterval(callBoth, 1000/framesPerSecond);
            
            //calling above code for paddle movement
            canvas.addEventListener('mousemove',
                function(evt) {
                var mousePos = calculateMousePos(evt);
                paddle1Y = mousePos.y-(PADDLE_HEIGHT/2);
            });
            
        }
        
        function ballReset() {
                ballSpeedX = -ballSpeedX
                ballX = canvas.width/2;
                ballY = canvas.height/2;
            }
        
        //code for "AI"
        function computerMovement(){
            var paddle2YCenter = paddle2Y + (PADDLE_HEIGHT/2);
            if(paddle2YCenter < ballY-35) {
                //paddle2Y = paddle2Y + 6;
                paddle2Y += 6;
            }else if(paddle2YCenter > ballY+35){
                //paddle2Y = paddle2Y - 6;
                paddle2Y -= 6;
            }
        }
        
        //movement code;
        function moveEverything(){
               //calling above function;
               computerMovement();
            
               ballX = ballX + ballSpeedX;
               ballY = ballY + ballSpeedY;
            
            //big scary code below is for ball movement;
            if(ballX < 0){
                if(ballY > paddle1Y &&
                   ballY < paddle1Y+PADDLE_HEIGHT){
                        ballSpeedX = -ballSpeedX;
                    
                        //code for vertical ball movement;
                        var deltaY = ballY
                                -(paddle1Y+PADDLE_HEIGHT/2);
                        ballSpeedY = deltaY * 0.35
                } 
                else {
                         //player2Score += 1; or
                         player2Score ++; //must be BEFORE ball reset for winning
                         ballReset();
                }
            }
            if(ballX > canvas.width){
                if(ballY > paddle2Y &&
                   ballY < paddle2Y+PADDLE_HEIGHT){
                        ballSpeedX = -ballSpeedX;
                    
                          //code for vertical ball movement;
                          var deltaY = ballY
                                -(paddle2Y+PADDLE_HEIGHT/2);
                        ballSpeedY = deltaY * 0.35
                } 
                else {
                         //player1Score += 1; or
                         player1Score ++; //must be BEFORE ball reset for winning
                         ballReset();
                }
            }
            if(ballY < 0){
                ballSpeedY = -ballSpeedY;
            }
            if(ballY > canvas.height){
                ballSpeedY = -ballSpeedY;
            }
        }
        
        //drawing code
        function drawEverything() {           
            //this is the canvas code
            colorRect(0,0,canvas.width,canvas.height,'black');
            //this is the left player paddle
            colorRect(0,paddle1Y,10,PADDLE_HEIGHT,'white');
            //this is the right player paddle
            colorRect(canvas.width - 10,paddle2Y,PADDLE_THICKNESS,PADDLE_HEIGHT,'white');
            //this is the ball
            colorCircle(ballX, ballY, 10,'white');
            
            //code for player scores
            canvasContext.fillText(player1Score, 100,100);
            canvasContext.fillText(player2Score, canvas.width - 100,100);
            
        
        }
        
        //helper function for colorCircle in function drawEverything above
        function colorCircle(centerX,centerY,radius,drawColor){
            canvasContext.fillStyle = 'drawColor';
            canvasContext.beginPath();
            canvasContext.arc(centerX, centerY, radius, 0, Math.PI*2,true);
            canvasContext.fill();
        }
        //helper function for colorRect in function drawEverything above
        function colorRect(leftX,topY,width,height,drawColor){
            canvasContext.fillStyle=drawColor;
            canvasContext.fillRect(leftX,topY,width,height,);
            
        }
        
        //code for var framesPerSecond within canvas code;
        function callBoth(){
            moveEverything();
            drawEverything();
        }
        
             
    </script>
    
</html>
