# clone-coding, pycar
     import pygame
     import random
     from time import sleep

    #화면크기
    window_width = 480
    window_height = 800

    #색깔
    black = (0, 0, 0)
    white = (255, 255, 255)
    gray = (150, 150, 150)
    red = (255, 0, 0)

    #객체
    class Car:
        image_car = ['RacingCar01.png','RacingCar02.png','RacingCar03.png',\
                     'RacingCar04.png','RacingCar05.png','RacingCar04.png',\
                     'RacingCar07.png','RacingCar08.png','RacingCar09.png',\
                     'RacingCar10.png','RacingCar11.png','RacingCar12.png',\
                     'RacingCar13.png','RacingCar14.png','RacingCar15.png',\
                     'RacingCar16.png','RacingCar17.png','RacingCar18.png',\
                     'RacingCar19.png','RacingCar20.png'] #리소스가져오기

        #초기화
        def __init__(self, x=0, y=0, dx=0, dy=0): #자동차 좌표값(d=어디로가는지)
            self.image = ""
            self.x = x
            self.y = y
            self.dx = dx
            self.dy = dy

        def load_image(self):
            self.image = pygame.image.load(random.choice(self.image_car))
            self.width = self.image.get_rect().size[0]
            self.height = self.image.get_rect().size[1]
            #자동차랜덤선택&자동차높이,너비

        def draw_image(self):
            screen.blit(self.image, [selfx, self.y])

        def move_x(self):
            self.x +=self.dx

        def move_y(self):
            self.y  +=self.dy

        def check_out_of_screen(self): #화면 넘어가는지 체크
            if self.x + self.width > window_width or self.x < 0:
                self.x -= self.dx

        def check_crash(self, car): #내 자동차와 다른 차가 부딪혔는지 확인
            if (self.x + self.width > car.x) and (self.x < car.x + car.width) \n
            and (self.y < car.y + car.height) and (self.y + self.height > car.y):
                return True #다른차와 겹침
            else:
                return False #(True, False 쓸 때 대문자쓰기)
    def draw_main_menu():
        draw_x = (window_width / 2)-200
        draw_y = window_height / 2
        image_intro = pygame.image.load('PyCar.png)
        screen.blit(image_intro, [draw_x, draw_y - 200])
        font_40 = pygame.font.SysFont("FixedSys", 40, True, False)
        font_30 = pygame.font.SysFont("FixedSys", 30, True, False)                                
        text_title = font_40.render("PyCar: Racing Car Game", True, BLACK)
        screen.blit(text_title, [draw_x, draw_y])
        text_score = font_40.render("Score: "+str(score), True, WHITE)
        screen.blit(text_title, [draw_x, draw_y + 70])
        text_start = font_30.render("Press Space Key to Start!", True, RED)
        screen.blit(text_title, [draw_x, draw_y + 140])
        pygame.display.flip()

    def draw_score():
        font_30 = pygame.font.SysFont("FixedSys", 30, True, False)
        text_score = font_40.render("Score: "+str(score), True, BLACK)
        screen.blit(text_score,[15,15])
        

    if __name__== '__main__':

        pygame.init()

        screen = pygame.display.set_mode(window_width, window_height)
        pygame.display.set_caption("PyCar:Racing Car Game")
        clock = pygame.time.Clock()

        pygame.mixer.music.load('race.wav')
        sound_crash = pygame.mixer.Sound('crash.wav')
        sound_engine = pygame.mixer.Sound('engine.wav')

        #초기 자동차 위치
        player = car(window_width / 2), (window_height-150,0,0)
        player.load_image()

        #다른자동차 랜덤 출몰
        cars = []
        car_count = 3
        for i in range(car_count):
            x = random.randrange(0, window_width-55)
            y = random.randrange(-150, -50)
            car = car(x, y, 0, random.randint(5, 10))
            car.load_image()
            cars.append(car)
        #차선움직임
        lanes = []
        lane_width = 10
        lane_height = 80
        lane_margin = 20
        lane_count = 20
        lane_x = (window_width - lane_width)/2
        lane_y = -10
        for i in range(mane_count):
            lanes.append([lane_x, lane_y])
            lane_y +=lane_height + lane_margin

        score = 0
        crash = True
        game_on = True
        while game_on:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    game_on = False

                if crash:
                    if event.type == pygame.KEYDOWN and event.key==pygame.K_SPACE:
                        crash = False
                        for i in range(car_count):
                            cars[i].x = random.randrange(0, window_width-car[i].width)
                            cars[i].y = random.randrange(-150, -50)
                            cars[i].load_image()

                        player.load_image()
                        player.x = window_width / 2
                        player.dx = 0
                        score = 0
                        pygame.mouse.set_visible(False)
                        sound_engine.play()
                        sleep(5)
                        pygame.mixer.music.play(-1)#-1의 의미는 사운드가 끝나도 다시 재생

                if not crash:
                    if event.type == pygame.KEYDOWN: #키다운-눌렀을때
                        if evevt.key == pygame.K_RIGHT:
                            player.dx = 4
                        elif event.key == pygame.K_LEFT:
                            player.dx = -4

                
                    if event.type == pygame.KEYDOWN:#키업-안눌렀을때(움직이면 안되니까0)
                        if evevt.key == pygame.K_RIGHT:
                            player.dx = 0
                        elif event.key == pygame.K_LEFT:
                            player.dx = 0

            #화면만들기 
            screen.fill(GRAY)

            #안부딪혔을때-계속 차선나와야함
            if not crash:
                for i in range(lane_count):
                    pygame.draw.rect(screen, WHITE, [lane[i][0], lane[i][1], \n
                                                 lane_width, lane_height])
                    lanes[i][1] += 10 #속도
                    if lanes[i][1] > window_height:
                        lanes[i][1] = -40 -lane_height

                player.draw_image()
                player.move_x()
                player.check_out_of_screen()
 
                for i in range(car_count):
                    cars[i].draw_image()
                    cars[i].y += cars[i].dy
                    if cars[i].y > window_height:
                        score += 10
                        cars[i].x = random.randrange(0, window_width-car[i].width)
                        cars[i].y = random.randrange(-150, -50)
                        cars[i].dy = random.randint(5, 10)
                        cars[i].load_image()

                for i in range(car_count):
                    if player_check_crash(cars[i]):
                        crash = True
                        pygame.mixer.music.stop()
                        sound_crash.play()
                        sleep(2)
                        pygame.mouse.set_visible(True)
                        break
            else:
                draw_main_menu()
            clock.tick(60)
    pygame.quit()

