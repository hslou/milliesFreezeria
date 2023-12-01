from cmu_graphics import *
import random, math

class Order:
    def __init__(self, orderNum, flavor, syrup, mix, drizzle, toppings, numToppings):
        self.orderNum = orderNum
        self.flavor = flavor
        self.syrup = syrup
        self.mix = mix
        self.wc = True
        self.drizzle = drizzle
        self.toppings = toppings
        self.numToppings = numToppings

    def __eq__(self, other):
        if isinstance(other, Drink):
            if ( (self.flavor == other.flavor) and (self.syrup == other.syrup) and
            (self.mix == other.mix) and (other.wc == True) and (self.drizzle == other.drizzle) and
            (self.toppings == other.toppings) and (self.numToppings == other.numToppings)):
                return True
        else:
            return False
        
    def __hash__(self):
        return hash(str(self.orderNum))
    
    def __repr__(self):
        return f'Drink #{self.orderNum}: flavor: {self.flavor}, syrup: {self.syrup}, mix: {self.mix}, drizzle: {self.drizzle}, toppings: {self.toppings}, numToppings: {self.numToppings}'
    
class Drink:
    def __init__(self, order, flavor, syrup, mix, drizzle):
        self.order = order
        self.flavor = flavor
        self.syrup = syrup
        self.mix = mix
        self.wc = True
        self.drizzle = drizzle
        self.toppings = []

    def __eq__(self, other):
        if isinstance(other, Order):
            if ( (self.flavor == other.flavor) and (self.syrup == other.syrup) and
            (self.mix == other.mix) and (other.wc == True) and (self.drizzle == other.drizzle) and
            (self.toppings == other.toppings) ):
                return True
        else:
            return False
        
    def __hash__(self):
        return hash(str(self))   

def takeOrder(app, orderNum):
    flavor = random.choice(app.flavors)
    syrup = random.choice(app.syrups)
    mix = random.choice(app.mix)
    drizzle = random.choice(app.drizzles)
    toppings = [random.choice(app.toppingsA), random.choice(app.toppingsB)]
    numToppings = random.choice(app.numToppings)
    order = Order(orderNum, flavor, syrup, mix, drizzle, toppings, numToppings)
    return order

def compareOrder(order, drink):
    if order == drink:
        return True
    else:
        return False
    
def onAppStart(app):
    app.flavors = ["choc", "strawberry", "matcha", "grape", "pb", "blueberry", "banana", "cookie"]
    app.syrups = ["strawberry", "choc", "vanilla", "banana"]
    app.mix = ["25", "50", "75", "100"]
    app.toppingsA = ["oreoCookie", "cherry"]
    app.toppingsB = ["chocChips", "sprinkles"]
    app.numToppings = [1, 2, 3]
    app.drizzles = ["strawDrizzle", "chocDrizzle"]
    app.orderNum = 0

    app.order = None
    app.score = 0
    app.drink = Drink(app.order, "", "", "", "")

    app.startGameScreen = True
    app.orderScreen = False
    app.orderTaken = False
    app.buildMilkStation = False
    app.isPouring = False
    app.isFilling = False
    app.milkPoured = False
    app.buildFlavorStation = False
    app.isPouringFlavor = False
    app.flavorSelected = False
    app.flavorPoured = False
    app.buildSyrupStation = False
    app.syrupSelected = False
    app.isPouringSyrup = False
    app.isFillingSyrup = False
    app.syrupPoured = False
    app.mixStation = False
    app.drinkInMixer = False
    app.drinkRemoved = False
    app.mixed = False
    app.toppingsStation = False
    app.compareStation = False

    app.beeperX = 176
    app.beeperXHitBottom = True
    app.stepsPerSecond = 60
    app.pouringLiquidLevel = 0
    app.pouringSyrupLevel = 0
    app.fillingY = 0
    app.fillingX = 0
    app.circles = []
    app.cX = 215
    app.cY = 250
    app.syrupX = 0
    app.syrupY = 0
    app.mixLevel = 0
    app.syrupWidth = 4
    app.syrupR = 0
    app.syrupG = 0
    app.syrupB = 0
    app.milkR = 255
    app.milkG = 249
    app.milkB = 220
    app.flavorRadius = 5

    app.counter = 1
    app.wcSelected = False
    app.strawbDrizzSelected = False
    app.chocDrizzSelected = False
    app.isDragging = False
    app.wcX = 330
    app.wcY = 60
    app.sdX = 60
    app.sdY = 60
    app.cdX = 100
    app.cdY = 60
    app.wcRotateAngle = 0
    app.sdRotateAngle = 0
    app.cdRotateAngle = 0

    app.wcPouring = False
    
    app.wcParticleX = 0
    app.wcParticleY = 0
    app.wcParticles = []
    app.fallenWCParticles = {}

def initFallenWCParticles():
    d = dict()
    for num in range(1, 46):
        d[num] = 0
    return d

def distance(x0, y0, x1, y1):
    return ((x1 - x0)**2 + (y1 - y0)**2)**0.5

def drawOrder(app):
    drawRect(450, 50, 200, 400, border="black", fill="white")
    drawImage(app.order.flavor+".png", 475, 395, width=50, height=50)
    drawImage(app.order.syrup+".png", 580, 395, width=50, height=50)
    drawLabel("flavor", 500, 385)
    drawLabel("syrup", 605, 385)
    drawLine(450, 375, 650, 375)
    drawImage(app.order.mix+".png", 465, 335, width=170, height=30)
    drawLabel("mix", 550, 325)
    drawLine(450, 315, 650, 315)
    drawImage("wc.png", 465, 275, width=170, height=30)
    drawLabel("whipped cream", 550, 265)
    drawLine(450, 255, 650, 255)
    drawImage(app.order.drizzle+".png", 465, 220, width=170, height=30)
    drawLabel("drizzle", 550, 210)
    drawLine(450, 200, 650, 200)
    drawImage(app.order.toppings[1]+".png", 530, 150, width=40, height=40)
    drawLabel("sprinkles", 490, 170)
    drawLine(450, 140, 650, 140)
    for i in range(app.order.numToppings):
        drawImage(app.order.toppings[0]+".png", 480+(i*50), 90, width=40, height=40)
    drawLine(450, 80, 650, 80)
    drawLabel(f"Order #{app.orderNum}", 495, 65, size=20)

def drawMilkshake(app):
    #drawLine(176, 315, 275, 315)
    if (app.milkPoured == True) and (app.drinkInMixer == False):
        drawPolygon(190, 405, 260, 405, 270, 345, 180, 345, fill="cornSilk")
    if (app.flavorPoured == True) and (app.drinkInMixer == False):
        flavorColor = flavorColors(app)
        altFlavorColor = flavorAltColors(app)
        drawPolygon(270, 345, 180, 345, 177, 325, 273, 325, fill=altFlavorColor)
        cx = 185
        cy = 340
        while cx < 270:
            drawCircle(cx, cy, 5, fill=flavorColor)
            cx += 10
            if cy == 340:
                cy = 330
            else:
                cy = 340
    if (app.syrupPoured == True) and (app.drinkInMixer == False):
        syrupColor = syrupColors(app)
        drawRect(176, 315, 97, 10, fill = syrupColor)
    if (app.drinkInMixer) or (app.mixed):
        drawPolygon(190, 405, 260, 405, 275, 315, 176, 315, 
                    fill=rgb(math.ceil(app.milkR), math.ceil(app.milkG), 
                             math.ceil(app.milkB)))
        if app.syrupWidth > 0 and app.mixLevel < 93:
            drawLine(210, 405, 187, 380, lineWidth=app.syrupWidth, 
                     fill=rgb(math.ceil(app.syrupR), math.ceil(app.syrupG), 
                              math.ceil(app.syrupB)))
            drawLine(240, 405, 180, 345, lineWidth=app.syrupWidth,
                     fill=rgb(math.ceil(app.syrupR), math.ceil(app.syrupG), 
                              math.ceil(app.syrupB)))
            drawLine(187, 317, 263, 390, lineWidth=app.syrupWidth,
                     fill=rgb(math.ceil(app.syrupR), math.ceil(app.syrupG), 
                              math.ceil(app.syrupB)))
            drawLine(222, 317, 267, 360, lineWidth=app.syrupWidth,
                     fill=rgb(math.ceil(app.syrupR), math.ceil(app.syrupG), 
                              math.ceil(app.syrupB)))
        drawMixCircles(app)
    drawLine(170, 285, 190, 405)
    drawLine(190, 405, 260, 405)
    drawLine(260, 405, 280, 285)
    #drawArc(225, 285, 110, 15, 0, 180, fill=None, border="black", rotateAngle=180)
    #drawLine()
    drawOval(225, 285, 110, 15, fill=None, border="black")

def drawBuildMilk(app):
    drawRect(150, 50, 150, 150, fill="silver")
    drawRect(200, 200, 50, 50, fill="silver")
    drawBeeper(app, 175, 175)
    drawCircle(225, 225, 20, fill="darkSeaGreen", border="cadetBlue")

def drawBeeper(app, x, y):
    drawRect(x, y, 10, 20, fill="lightCoral")
    drawRect(x+10, y, 30, 20, fill="khaki")
    drawRect(x+40, y, 20, 20, fill="darkSeaGreen")
    drawRect(x+60, y, 30, 20, fill="khaki")
    drawRect(x+90, y, 10, 20, fill="lightCoral")
    drawRect(app.beeperX, y, 4, 20, fill="cadetBlue")

def pour(app):
    if (app.beeperX > 215) and (app.beeperX < 235):
        app.score += 10 #green
    elif (app.beeperX < 185) or (app.beeperX > 275):
        pass #red
    else: 
        app.score += 5 #yellow

def checkSelection(app, ingredient):
    if ingredient == "flavor":
        if app.drink.flavor == app.order.flavor:
            app.score += 10
    if ingredient == "syrup":
        if app.drink.syrup == app.order.syrup:
            app.score += 10

def drawFlavorSelection(app):
    drawRect(40, 50, 115, 150, fill="silver")
    drawRect(300, 50, 115, 150, fill="silver")
    drawImage("strawberry.png", 45, 70, width=50, height=50)
    drawImage("banana.png", 45, 125, width=50, height=50)
    drawImage("matcha.png", 100, 70, width=50, height=50)
    drawImage("cookie.png", 100, 125, width=50, height=50)
    drawImage("blueberry.png", 305, 70, width=50, height=50)
    drawImage("choc.png", 305, 125, width=50, height=50)
    drawImage("pb.png", 360, 70, width=50, height=50)
    drawImage("grape.png", 360, 125, width=50, height=50)
    drawLabel("choose flavor", 225, 125)

def flavorColors(app):
    if app.drink.flavor == "strawberry":
        return "lightPink"
    elif app.drink.flavor == "banana":
        return "khaki"
    elif app.drink.flavor == "matcha":
        return "oliveDrab"
    elif app.drink.flavor == "cookie":
        return "peru"
    elif app.drink.flavor == "blueberry":
        return "cornflowerBlue"
    elif app.drink.flavor == "choc":
        return "saddleBrown"
    elif app.drink.flavor == "pb":
        return "chocolate"
    elif app.drink.flavor == "grape":
        return "mediumPurple"
    
def flavorAltColors(app):
    if app.drink.flavor == "strawberry":
        return "mistyRose"
    elif app.drink.flavor == "banana":
        return "ivory"
    elif app.drink.flavor == "matcha":
        return "darkSeaGreen"
    elif app.drink.flavor == "cookie":
        return "tan"
    elif app.drink.flavor == "blueberry":
        return "aliceBlue"
    elif app.drink.flavor == "choc":
        return "antiqueWhite"
    elif app.drink.flavor == "pb":
        return "blanchedAlmond"
    elif app.drink.flavor == "grape":
        return "thistle"

def drawFlavorPouring(app):
    color = flavorColors(app)
    for (circleX, circleY) in app.circles: 
        drawCircle(circleX, circleY, 5, fill=color)

def drawSyrupSelection(app):
    drawRect(40, 50, 57.5, 200, fill="lightPink")
    drawImage("strawberry.png", 43, 140, width=50, height=50)
    drawRect(97.5, 50, 57.5, 200, fill="cornSilk")
    drawImage("vanilla.png", 100.5, 140, width=50, height=50)
    drawRect(300, 50, 57.5, 200, fill="saddleBrown")
    drawImage("choc.png", 300.5, 140, width=50, height=50)
    drawRect(357.5, 50, 57.5, 200, fill="khaki")
    drawImage("banana.png", 360.5, 140, width=50, height=50)
    drawLabel("choose flavor", 225, 125)

def syrupColors(app):
    if app.drink.syrup == "strawberry":
        return "paleVioletRed"
    elif app.drink.syrup == "vanilla":
        return "wheat"
    elif app.drink.syrup == "choc":
        return "sienna"
    elif app.drink.syrup == "banana":
        return "gold"

def getFlavor(app, mouseX, mouseY):
    if (mouseX < 95) and (mouseX > 45) and (mouseY < 120) and (mouseY > 70):
        app.drink.flavor = "strawberry"
        app.flavorSelected = True
        #print("strawberry")
    elif (mouseX < 95) and (mouseX > 45) and (mouseY < 175) and (mouseY > 125):
        app.drink.flavor = "banana"
        app.flavorSelected = True
        #print("banana")
    elif (mouseX < 150) and (mouseX > 100) and (mouseY < 120) and (mouseY > 70):
        app.drink.flavor = "matcha"
        app.flavorSelected = True
        #print("matcha")
    elif (mouseX < 150) and (mouseX > 100) and (mouseY < 175) and (mouseY > 125):
        app.drink.flavor = "cookie"
        app.flavorSelected = True
        #print("cookie")
    elif (mouseX < 355) and (mouseX > 305) and (mouseY < 120) and (mouseY > 70):
        app.drink.flavor = "blueberry"
        app.flavorSelected = True
        #print("pb")
    elif (mouseX < 355) and (mouseX > 305) and (mouseY < 175) and (mouseY > 125):
        app.drink.flavor = "choc"
        app.flavorSelected = True
        #print("choc")
    elif (mouseX < 410) and (mouseX > 360) and (mouseY < 120) and (mouseY > 70):
        app.drink.flavor = "pb"
        app.flavorSelected = True
        #print("oreo")
    elif (mouseX < 410) and (mouseX > 360) and (mouseY < 175) and (mouseY > 125):
        app.drink.flavor = "grape"
        app.flavorSelected = True
        #print("grape")

def getSyrup(app, mouseX, mouseY):
    if (mouseX < 73) and (mouseX > 43) and (mouseY < 190) and (mouseY > 140):
        app.drink.syrup = "strawberry"
        app.syrupSelected = True
        print("strawberry")
    elif (mouseX < 150.5) and (mouseX > 100.5) and (mouseY < 190) and (mouseY > 140):
        app.drink.syrup = "vanilla"
        app.syrupSelected = True
        print("vanilla")
    elif (mouseX < 350.5) and (mouseX > 300.5) and (mouseY < 190) and (mouseY > 140):
        app.drink.syrup = "choc"
        app.syrupSelected = True
        print("choc")
    elif (mouseX < 410.5) and (mouseX > 360.5) and (mouseY < 190) and (mouseY > 140):
        app.drink.syrup = "banana"
        app.syrupSelected = True
        print("banana")

def drawMix(app):
    drawRect(150, 50, 150, 150, fill="silver")
    drawRect(165, 150, 124, 20, border="black", fill="white")
    if app.mixLevel > 0:
        syrupColor = syrupColors(app)
        drawRect(165, 150, app.mixLevel, 20, border="black", fill="mediumPurple")
    drawLine(196, 150, 196, 170)
    drawLine(227, 150, 227, 170)
    drawLine(258, 150, 258, 170)

def runMixer(app):
    if app.order.mix == "25":
        if (app.mixLevel < 201) and ((app.mixLevel+165) > 191):
            app.score += 10
    if app.order.mix == "50":
        if (app.mixLevel < 67) and (app.mixLevel > 57):
            app.score += 10
    if app.order.mix == "75":
        if (app.mixLevel < 98) and (app.mixLevel > 88):
            app.score += 10
    if app.order.mix == "100":
        if (app.mixLevel == 124):
            app.score += 10

def calcMixColors(app):
    if app.drink.syrup == "strawberry":
        app.syrupR = 219
        app.syrupG = 112
        app.syrupB = 147
    elif app.drink.syrup == "vanilla":
        app.syrupR = 245
        app.syrupG = 222
        app.syrupB = 179
    elif app.drink.syrup == "choc":
        app.syrupR = 160
        app.syrupG = 82
        app.syrupB = 45
    elif app.drink.syrup == "banana":
        app.syrupR = 255
        app.syrupG = 215
        app.syrupB = 0

def drawMixCircles(app):
    flavorColor = flavorColors(app)
    drawLine(176, 315, 275, 315)
    cx = 185
    cy = 325
    dcx = 0
    while cy < 395:
        while cx < (260-dcx):
            drawCircle(cx+dcx, cy, math.ceil(app.flavorRadius), fill=flavorColor)
            cx += 20
        cy += 15
        dcx += 5
        cx = 185 + dcx

def drawToppingsOptions(app):
    drawImage("oreoCookie.png", 70, 555, width=50, height=50)
    drawImage("cherry.png", 70, 475, width=50, height=50)
    drawCircle(95, 580, 30, fill=None, border="black")
    drawCircle(95, 500, 30, fill=None, border="black")
    drawImage("sprinkles.png", 320, 550, width=60, height=60)
    drawImage("chocChips.png", 320, 470, width=60, height=60)
    #whipped cream and drizzle bottles
    drawImage("wcBottle.png", app.wcX, app.wcY, width=30, height=120, 
              rotateAngle=app.wcRotateAngle)
    drawImage("stawbDrizzleBottle.png", app.sdX, app.sdY, width=30, height=120,
              rotateAngle=app.sdRotateAngle)
    drawImage("chocDrizzleBottle.png", app.cdX, app.cdY, width=30, height=120, 
              rotateAngle=app.cdRotateAngle)

def redrawAll(app):
    if app.startGameScreen:
        drawLabel("Millie's Freezeria", 350, 250, size=50)
        drawLabel("Take customer orders and build their milkshakes!", 350, 300)
        drawLabel("Press enter to play", 350, 350)
    elif app.orderScreen:
        drawLabel("Order Station", 100, 25, size=20)
        drawCircle(100, 500, 50, fill="darkSeaGreen")
        drawLabel("take order", 100, 500)
        if app.orderTaken:
            drawOrder(app)
            drawCircle(100, 500, 50, fill="lightCoral")
            drawLabel("order taken", 100, 500)
            drawCircle(300, 500, 50, fill="darkSeaGreen")
            drawLabel("continue", 300, 500)
    elif app.buildMilkStation:
        drawOrder(app)
        drawBuildMilk(app)
        drawMilkshake(app)
        drawLabel("Build Station - Step 1: Milk", 150, 25, size=20)
        if app.isPouring and (app.pouringLiquidLevel > 0):
            drawRect(205, 250, 40, app.pouringLiquidLevel, fill="cornSilk")
            drawMilkshake(app)
        if app.isFilling and (app.fillingY > 0):
            drawPolygon(190, 405, 260, 405, 260+app.fillingX, 405-app.fillingY, 
                        190-app.fillingX, 405-app.fillingY, fill="cornSilk")
            drawMilkshake(app)
        if app.milkPoured:
            drawMilkshake(app)
            drawCircle(300, 500, 50, fill="darkSeaGreen")
            drawLabel("continue", 300, 500)
    elif app.buildFlavorStation:
        drawOrder(app)
        drawFlavorSelection(app)
        drawMilkshake(app)
        drawLabel("Build Station - Step 2: Flavor", 150, 25, size=20)
        if app.flavorSelected:
            drawBuildMilk(app)
            if app.isPouringFlavor and (app.cY > 254):
                drawFlavorPouring(app)
            if app.flavorPoured:
                drawMilkshake(app)
                drawCircle(300, 500, 50, fill="darkSeaGreen")
                drawLabel("continue", 300, 500)
    elif app.buildSyrupStation:
        drawOrder(app)
        drawSyrupSelection(app)
        drawMilkshake(app)
        drawLabel("Build Station - Step 3: Syrup", 150, 25, size=20)
        if app.syrupSelected:
            drawBuildMilk(app)
            syrupColor = syrupColors(app)
            if app.isPouringSyrup and app.syrupY > 0: 
                drawRect(225, 250, 5, app.syrupY, fill=syrupColor)
                drawMilkshake(app)
            if app.isFillingSyrup and app.syrupX > 0:
                drawRect(225-app.syrupX, 315, app.syrupX, 10, fill=syrupColor)
                drawRect(225, 315, app.syrupX, 10, fill=syrupColor)
                drawMilkshake(app)
            if app.syrupPoured:
                drawMilkshake(app)
                drawCircle(300, 500, 50, fill="darkSeaGreen")
                drawLabel("continue", 300, 500)
    elif app.mixStation:
        drawOrder(app)
        drawMilkshake(app)
        drawLabel("Mix Station - Step 5: Mix", 150, 25, size=20)
        drawCircle(100, 500, 50, fill="darkSeaGreen")
        drawLabel("place in mixer", 100, 500)
        drawMix(app)
        if app.drinkInMixer:
            drawCircle(200, 500, 50, fill="khaki")
            drawLabel("remove from mixer", 200, 500)
        if app.mixed:
            drawCircle(200, 500, 50, fill="lightCoral")
            drawLabel("drink removed", 200, 500)
            drawCircle(300, 500, 50, fill="darkSeaGreen")
            drawLabel("continue", 300, 500)
    elif app.toppingsStation:
        drawOrder(app)
        drawMilkshake(app)
        drawGrid(app)
        drawLine(0, 270, 450, 270)
        drawLabel("Toppings Station - Step 6: Toppings", 170, 25, size=20)
        drawCircle(500, 500, 50, fill="darkSeaGreen")
        drawLabel("continue", 500, 500)
        drawToppingsOptions(app)
        if app.wcPouring:
            for particle in app.wcParticles:
                drawRect(particle[0], particle[1], 10, 10, fill=None, 
                         border="lightBlue")
            #print(app.fallenWCParticles)
            for xCoord in app.fallenWCParticles:
                yCoord = app.fallenWCParticles[xCoord]
                #print(xCoord)
                if (xCoord >= 170) and (xCoord <= 280):
                    for y in range(yCoord, 305, 10):
                        drawRect(xCoord, y, 10, 10, fill=None, border="lightBlue")
                if (xCoord < 170) or (xCoord > 280):
                    for y in range(yCoord, 395, 10):
                        drawRect(xCoord, y, 10, 10, fill=None, border="lightBlue")

def onKeyPress(app, key):
    if app.startGameScreen and (key == "enter"):
        app.startGameScreen = False
        app.orderScreen = True

def onMousePress(app, mouseX, mouseY):
    #take order button
    if app.orderScreen and (distance(mouseX, mouseY, 100, 500) <= 50) and (app.orderTaken == False):
        app.order = takeOrder(app, app.orderNum)
        app.orderTaken = True
        app.orderNum += 1
        print(app.order)
    #continue from order to build milk station
    if app.orderScreen and (distance(mouseX, mouseY, 300, 500) <= 50) and (app.orderTaken == True):
        app.orderScreen = False
        app.buildMilkStation = True
    #build milk station
    if app.buildMilkStation and (distance(mouseX, mouseY, 225, 225) <= 20) and (app.milkPoured == False):
        pour(app)
        app.isPouring = True
        if app.isPouring == False:
            app.milkPoured = True
        print("score: ", app.score)
    #continue from build milk to build flavor station
    if app.buildMilkStation and (distance(mouseX, mouseY, 300, 500) <= 50) and (app.milkPoured == True):
        app.buildMilkStation = False
        app.buildFlavorStation = True
    #build flavor station - flavor selection
    if app.buildFlavorStation and (app.flavorSelected == False):
        getFlavor(app, mouseX, mouseY)
        checkSelection(app, "flavor")
        print("score: ", app.score)
    #build flavor station - pour flavor
    if app.buildFlavorStation and (distance(mouseX, mouseY, 225, 225) <= 20) and (app.flavorPoured == False):
        pour(app)
        app.isPouringFlavor = True
        if app.isPouringFlavor == False:
            app.flavorPoured = True
        print("score: ", app.score)
    #continue from build flavor to build syrup station
    if app.buildFlavorStation and (distance(mouseX, mouseY, 300, 500) <= 50) and (app.flavorPoured == True):
        app.buildFlavorStation = False
        app.buildSyrupStation = True
    #build syrup station - syrup selection
    if app.buildSyrupStation and (app.syrupSelected == False):
        getSyrup(app, mouseX, mouseY)
        checkSelection(app, "syrup")
        print("score: ", app.score)
    #build syrup station - pour syrup
    if app.buildSyrupStation and (distance(mouseX, mouseY, 225, 225) <= 20) and (app.syrupPoured == False):
        pour(app)
        app.isPouringSyrup = True
        if app.isPouringSyrup == False:
            app.syrupPoured = True 
        print("score: ", app.score)
    #continue from build syrup to mix station
    if app.buildSyrupStation and (distance(mouseX, mouseY, 300, 500) <= 50) and (app.syrupPoured == True):
        app.buildSyrupStation = False
        app.mixStation = True
    #place in mix station
    if app.mixStation and (distance(mouseX, mouseY, 100, 500) <= 50) and (app.drinkInMixer == False):
        app.drinkInMixer = True
        runMixer(app)
    #remove from mix station
    if app.mixStation and (distance(mouseX, mouseY, 200, 500) <= 50) and (app.drinkInMixer == True):
        app.drinkInMixer = False
        app.mixed = True
    #continue from mix station to toppings station
    if app.mixStation and (distance(mouseX, mouseY, 300, 500) <= 50) and (app.mixed == True):
        app.mixStation = False
        app.toppingsStation = True
    #toppings station
    if app.toppingsStation and (app.isDragging == False):
        if (mouseX > app.wcX) and (mouseX < app.wcX+30) and (mouseY > app.wcY) and (mouseY < app.wcY+120):
            app.wcSelected = True
            app.isDragging = True
        if (mouseX > app.sdX) and (mouseX < app.sdX+30) and (mouseY > app.sdY) and (mouseY < app.sdY+120):
            app.strawbDrizzSelected = True
            app.isDragging = True
        if (mouseX > app.cdX) and (mouseX < app.cdX+30) and (mouseY > app.cdY) and (mouseY < app.cdY+120):
            app.chocDrizzSelected = True
            app.isDragging = True
        
def onMouseDrag(app, mouseX, mouseY):
    if app.wcSelected:
        app.wcX = mouseX
        app.wcY = mouseY
        app.wcRotateAngle = 180
    elif app.strawbDrizzSelected:
        app.sdX = mouseX
        app.sdY = mouseY
        app.sdRotateAngle = 180
    elif app.chocDrizzSelected:
        app.cdX = mouseX
        app.cdY = mouseY
        app.cdRotateAngle = 180

def drawGrid(app):
    x = y = 0
    for i in range(45):
        drawLine(x, y, x, 270, fill="lightGray")
        x += 10
        #y += 10
    x = y = 0
    for j in range(27):
        drawLine(x, y, 450, y, fill="lightGray")
        #x += 10
        y += 10


def dropWhippedCream(app):
    app.wcParticles.append([app.wcParticleX+12, app.wcParticleY+120])
    particles = app.wcParticles
    removeParticles(app, particles)
    if app.wcParticles != []:
        for particle in app.wcParticles:
            particle[1] += 15

def removeParticles(app, particles):
    if particles != []:
        xCoord = particles[0][0]
        yCoord = particles[0][1]
        if ((xCoord//10)*10) in app.fallenWCParticles:
            yBound = app.fallenWCParticles[((xCoord//10)*10)]
        else:
            if (xCoord >= 170) and (xCoord <= 280):
                yBound = 305
            elif (xCoord < 170) or (xCoord > 280):
                yBound = 395
    elif particles == []:
        return
    #base case
    if (xCoord >= 170) and (xCoord <= 280) and (yCoord <= yBound):
        return particles
    #second base case
    elif ((xCoord < 170) or (xCoord > 280)) and (yCoord <= yBound):
        return particles
    else:
        if particles[0][1] > yBound:
            popped = particles.pop(0)
            if (((popped[0]//10)*10)) in app.fallenWCParticles:
                if app.fallenWCParticles[((popped[0]//10)*10)] > app.wcY+120:
                    app.fallenWCParticles[((popped[0]//10)*10)] -= 10
                    print((popped[0]//10)*10);
            else:
                app.fallenWCParticles[((popped[0]//10)*10)] = popped[1]
            removeParticles(app, particles)
 
    '''
    if (((app.wcParticleX+12 >= 170) and (app.wcParticleX+12 <= 280) and 
        (particles[0][1] <= 305)) or (((app.wcParticleX+12 < 170) or 
                                       (app.wcParticleX+12 > 280)) and particles[0][1] <= 395)):
        return particles
    else:
        if (app.wcParticleX+12 >= 170) and (app.wcParticleX+12 <= 280):
            if particles[0][1] > 305:
                popped = particles.pop(0)
        else:
            if particles[0][1] > 395:
                popped = particles.pop(0)
        if popped[0] in app.fallenWCParticles:
            app.fallenWCParticles[popped[0]] += 10
        else:
            app.fallenWCParticles[popped[0]] = popped[1]
        removeParticles(app, particles)
    '''
    

#def dropWhippedCreamGround(app):
 #  app.wcParticlesGround.append([app.wcParticleX+12, app.wcParticleY+120])

def onMouseMove(app, mouseX, mouseY):
    #app.mouseMoving = True
    if app.wcPouring:
        app.wcParticleX = mouseX
        app.wcParticleY = mouseY
        app.wcX = mouseX
        app.wcY = mouseY

def onMouseRelease(app, mouseX, mouseY):
    if app.wcSelected:
        app.wcPouring = True
    if app.wcPouring:
        app.wcParticleX = mouseX
        app.wcParticleY = mouseY
        app.wcX = mouseX
        app.wcY = mouseY
    #app.isDragging = False
    if app.wcPouring == False:
        app.wcSelected = False
        app.wcRotateAngle = 0
        app.wcX = 330
        app.wcY = 60
    app.strawbDrizzSelected = False
    app.chocDrizzSelected = False
    app.sdRotateAngle = 0
    app.cdRotateAngle = 0
    app.sdX = 60
    app.cdX = 100
    app.sdY = app.cdY = 60
    

def onStep(app):
    if ((app.buildMilkStation and (app.isPouring == False) and (app.isFilling == False) and (app.milkPoured == False)) or
        ((app.buildFlavorStation) and (app.isPouringFlavor == False) and (app.flavorPoured == False)) or
        ((app.buildSyrupStation == True) and (app.isPouringSyrup == False) and (app.isFillingSyrup == False) and 
         (app.syrupPoured == False))):
        if (app.beeperX > 175) and (app.beeperX < 275) and (app.beeperXHitBottom == True):
            app.beeperX += 5
        if app.beeperX >= 275 or app.beeperX <= 175:
            app.beeperXHitBottom = not app.beeperXHitBottom
            if app.beeperX >= 275:
                app.beeperX -= 5
            else:
                app.beeperX += 5
        if (app.beeperX > 175) and (app.beeperX < 275) and (app.beeperXHitBottom == False):
            app.beeperX -= 5
    if (app.buildMilkStation and app.isPouring) and (app.pouringLiquidLevel <= 150):
        app.pouringLiquidLevel += 8
    if app.pouringLiquidLevel > 150:
        app.isFilling = True
    if (app.buildMilkStation and app.isFilling) and (app.fillingY <= 60):
        app.fillingY += 1
        app.fillingX += (1/6)
        # print("app.fillingY: ", app.fillingY, "; app.fillingX: ", app.fillingX)
    if (app.buildMilkStation and app.fillingY > 60):
        app.milkPoured = True
    if app.milkPoured:
        app.isPouring = False
        app.isFilling = False
    if (app.buildFlavorStation and app.isPouringFlavor) and (app.cY <= 340):
        app.cY += 10
        app.circles.append((app.cX, app.cY))
        if app.cX == 215:
            app.cX += 20
        else:
            app.cX = 215
    if (app.cY > 340):
        app.isPouringFlavor = False
        app.flavorPoured = True
    if (app.buildSyrupStation and app.isPouringSyrup and app.syrupY <= 75):
        app.syrupY += 8
    if (app.buildSyrupStation and app.syrupY > 75):
        app.isFillingSyrup = True
    if (app.buildSyrupStation and app.isFillingSyrup and app.syrupX <= 47):
        app.syrupX += 5
    if app.buildSyrupStation and app.syrupX > 47:
        app.syrupPoured = True
    if app.syrupPoured:
        app.isPouringSyrup = False
        app.isFillingSyrup = False
    if (app.mixStation) and (app.drinkInMixer == True) and (app.mixed == False) and (app.mixLevel <= 124):
        app.mixLevel += 0.5
        calcMixColors(app)
        if app.mixLevel >= 0 and app.mixLevel <= 124:
            app.syrupWidth -= (4-1)/186
            app.flavorRadius -= (5-1.5)/248
            if app.drink.syrup == "choc":
                app.syrupR += (170-160)/186
                app.syrupG += (101-82)/186
                app.syrupB += (59-45)/186
                app.milkR -= (255-170)/248
                app.milkG -= (248-101)/248
                app.milkB -= (220-59)/248
            if app.drink.syrup == "banana":
                app.syrupG += (223-215)/186
                app.syrupB += 80/186
                app.milkG -= (248-223)/248
                app.milkB -= (220-80)/248
            if app.drink.syrup == "strawberry":
                app.syrupR += (236-219)/186
                app.syrupG += (142-112)/186
                app.syrupB -= (147-143)/186
                app.milkR -= (255-236)/248
                app.milkG -= (248-142)/248
                app.milkB -= (220-143)/248
            if app.drink.syrup == "vanilla":
                app.syrupR += (247-245)/186
                app.syrupG += (228-222)/186
                app.syrupB += (188-179)/186
                app.milkR -= (255-247)/248
                app.milkG -= (248-228)/248
                app.milkB -= (220-188)/248 
    if app.toppingsStation and app.wcPouring:
        dropWhippedCream(app)
        

        '''
        counter = 0
        dropWhippedCream(app)
        if (app.wcParticles[0][0] >= 176 and app.wcParticles[0][0] <= 275 
            and app.wcParticles[0][1] > 305) or ((app.wcParticles[0][0] < 176
            or app.wcParticles[0][0] > 275) and app.wcParticles[0][1] > 295): 
            app.wcParticles.pop(0) 
        print(counter, "hello")
        #dropWhippedCream(app)   
        if app.wcParticles != []:
            for particle in app.wcParticles:
                particle[1] += 15
        counter += 1
        '''

def main():
    runApp(width = 700, height = 600)

main()

#1. 10 points for scoring on filling milk
#2. 10 points for picking the right flavor
#3. 10 points for scoring on filling flavor
#4. 10 points for picking the right syrup
#5. 10 points for scoring on filling syrup
#6. 10 points for mixing right
#7. 10 points for whipped cream
#8. 10 points for the right drizzle
#9. 10 points for the right toppingsA
#10. 10 points for the right toppingsB
    
# 1. cornSilk (fff8dc) to paleVioletRed (db7093) 
#       end for 25: liquid (f4ae96) & stripe (ec8e8f)
#       end for 50: liquid & stripe (f09e91)
#       end for 75: liquid & stripe (ec8e8f)
#       end for 100: liquid & stripe (e57e90)
# 2. cornSilk (fff8dc) to wheat (f5deb3)
#       end for 25: liquid (f9eac5) & stripe (f6e1b7)
#       end for 50: liquid & stripe (f7e4bc)
#       end for 75: liquid & stripe (f6e1b7)
#       end for 100: liquid & stripe (f5deb3)
# 3. cornSilk (fff8dc) to sienna (a0522d)
#       end for 25: liquid (c89d6e) & stripe (aa653b)
#       end for 50: liquid & stripe (b4784a)
#       end for 75: liquid & stripe (aa653b)
#       end for 100: liquid & stripe (aa653b)
# 4. cornSilk (fff8dc) to gold (ffd700)
#       end for 25: liquid (#ffe77c) & stripe (#ffdb35)
#       end for 50: liquid & stripe (#ffdb35)
#       end for 75: liquid & stripe (#ffe367)
#       end for 100: liquid & stripe (#ffdb35)