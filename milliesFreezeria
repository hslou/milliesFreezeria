from cmu_graphics import *
import random

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
    app.flavors = ["choc", "strawberry", "matcha", "grape", "oreo", "blueberry", "banana", "cookie"]
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
    app.milkPoured = False
    app.buildFlavorStation = False
    app.flavorSelected = False
    app.flavorPoured = False
    app.buildSyrupStation = False
    app.syrupSelected = False
    app.syrupPoured = False
    app.mixStation = False
    app.drinkInMixer = False
    app.drinkRemoved = False
    app.mixed = False
    app.toppingsStation = False
    app.compareStation = False

    app.test = False 

    app.beeperX = 176
    app.beeperXHitBottom = True
    app.stepsPerSecond = 50
    app.mixLevel = 0

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
    drawImage("oreo.png", 360, 70, width=50, height=50)
    drawImage("grape.png", 360, 125, width=50, height=50)
    drawLabel("choose flavor", 225, 125)

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
        app.drink.flavor = "oreo"
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
        drawRect(165, 150, app.mixLevel, 20, border="black", fill="mediumPurple")
    drawLine(196, 150, 196, 170)
    drawLine(227, 150, 227, 170)
    drawLine(258, 150, 258, 170)

def runMixer(app):
    if app.order.mix == "25":
        if ((app.mixLevel+165) < 201) and ((app.mixLevel+165) > 190):
            app.score += 10
    if app.order.mix == "50":
        if ((app.mixLevel+165) < 232) and ((app.mixLevel+165) > 222):
            app.score += 10
    if app.order.mix == "75":
        if ((app.mixLevel+165) < 263) and ((app.mixLevel+165) > 253):
            app.score += 10
    if app.order.mix == "100":
        if (app.mixLevel == 124):
            app.score += 10

def redrawAll(app):
    if app.startGameScreen:
        drawLabel("Millie's Freezeria", 350, 250, size=50)
        drawLabel("Take customer orders and build their milkshakes!", 350, 300)
        drawLabel("Press enter to play", 350, 350)
    elif app.orderScreen:
        drawLabel("Order Station", 100, 25, size=20)
        drawCircle(100, 400, 50, fill="darkSeaGreen")
        drawLabel("take order", 100, 400)
        if app.orderTaken:
            drawOrder(app)
            drawCircle(100, 400, 50, fill="lightCoral")
            drawLabel("order taken", 100, 400)
            drawCircle(300, 400, 50, fill="darkSeaGreen")
            drawLabel("continue", 300, 400)
    elif app.buildMilkStation:
        drawOrder(app)
        drawBuildMilk(app)
        drawLabel("Build Station - Step 1: Milk", 150, 25, size=20)
        if app.milkPoured:
            drawCircle(300, 400, 50, fill="darkSeaGreen")
            drawLabel("continue", 300, 400)
    elif app.buildFlavorStation:
        drawOrder(app)
        drawFlavorSelection(app)
        drawLabel("Build Station - Step 2: Flavor", 150, 25, size=20)
        if app.flavorSelected:
            drawBuildMilk(app)
            if app.flavorPoured:
                drawCircle(300, 400, 50, fill="darkSeaGreen")
                drawLabel("continue", 300, 400)
    elif app.buildSyrupStation:
        drawOrder(app)
        drawSyrupSelection(app)
        drawLabel("Build Station - Step 3: Syrup", 150, 25, size=20)
        if app.syrupSelected:
            drawBuildMilk(app)
            if app.syrupPoured:
                drawCircle(300, 400, 50, fill="darkSeaGreen")
                drawLabel("continue", 300, 400)
    elif app.mixStation:
        drawOrder(app)
        drawLabel("Build Station - Step 4: Mix", 150, 25, size=20)
        drawCircle(100, 400, 50, fill="darkSeaGreen")
        drawLabel("place in mixer", 100, 400)
        drawMix(app)
        if app.drinkInMixer:
            drawCircle(200, 400, 50, fill="khaki")
            drawLabel("remove from mixer", 200, 400)
        if app.mixed:
            drawCircle(200, 400, 50, fill="lightCoral")
            drawLabel("drink removed", 200, 400)
            drawCircle(300, 400, 50, fill="darkSeaGreen")
            drawLabel("continue", 300, 400)


def onKeyPress(app, key):
    if app.startGameScreen and (key == "enter"):
        app.startGameScreen = False
        app.orderScreen = True

def onMousePress(app, mouseX, mouseY):
    #take order button
    if app.orderScreen and (distance(mouseX, mouseY, 100, 400) <= 50) and (app.orderTaken == False):
        app.order = takeOrder(app, app.orderNum)
        app.orderTaken = True
        app.orderNum += 1
        print(app.order)
    #continue from order to build milk station
    if app.orderScreen and (distance(mouseX, mouseY, 300, 400) <= 50) and (app.orderTaken == True):
        app.orderScreen = False
        app.buildMilkStation = True
    #build milk station
    if app.buildMilkStation and (distance(mouseX, mouseY, 225, 225) <= 20) and (app.milkPoured == False):
        pour(app)
        app.milkPoured = True
        print("score: ", app.score)
    #continue from build milk to build flavor station
    if app.buildMilkStation and (distance(mouseX, mouseY, 300, 400) <= 50) and (app.milkPoured == True):
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
        app.flavorPoured = True
        print("score: ", app.score)
    #continue from build flavor to build syrup station
    if app.buildFlavorStation and (distance(mouseX, mouseY, 300, 400) <= 50) and (app.flavorPoured == True):
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
        app.syrupPoured = True 
        print("score: ", app.score)
    #continue from build syrup to mix station
    if app.buildSyrupStation and (distance(mouseX, mouseY, 300, 400) <= 50) and (app.syrupPoured == True):
        app.buildSyrupStation = False
        app.mixStation = True
    #place in mix station
    if app.mixStation and (distance(mouseX, mouseY, 100, 400) <= 50) and (app.drinkInMixer == False):
        app.drinkInMixer = True
        runMixer(app)
    #remove from mix station
    if app.mixStation and (distance(mouseX, mouseY, 200, 400) <= 50) and (app.drinkInMixer == True):
        app.drinkInMixer = False
        app.mixed = True
    

def onStep(app):
    if (app.buildMilkStation and (app.milkPoured == False)) or ((app.buildFlavorStation) and (app.flavorPoured == False)) or ((app.buildSyrupStation) and (app.syrupPoured == False)):
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
    if (app.mixStation) and (app.drinkInMixer == True) and (app.mixed == False) and (app.mixLevel <= 124):
        app.mixLevel += 0.5
        

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
    
#lightPink for strawberry
#lemmonChiffon or lightYellow or cornSilk for vanilla
#khaki for banana
#saddleBrown for choc