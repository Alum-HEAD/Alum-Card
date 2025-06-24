/* 
REMISE DE DIPLOME 2025
PROJET ALUM-HEAD x Andrea Becze-Deak
PLATEFORME DE CUSTOMISATION
------------------------------------------------------------
VISUEL GENERATIF
------------------------------------------------------------
*/

class Tree {
    constructor(ctx, width, height, trunkColor, departmentZones, maxDepth = 20) {
        this.ctx = ctx;
        this.width = width;
        this.height = height;
        this.len = this.height * 0.15;
        this.angle = Math.random() * (0.5 - 0.2) + 0.2;
        this.fruitProb = 20;
        this.trunkColor = trunkColor;
        this.departmentZones = departmentZones;
        this.ctx.lineCap = "round";
        this.maxDepth = maxDepth;


        this.treeGradient = this.ctx.createLinearGradient(0, this.height, 0, 0);
        this.treeGradient.addColorStop(1, "rgb(35, 88, 11)");
        /*this.treeGradient.addColorStop(1, "rgb(21, 11, 88)");*/


        const MIN_REDUCTION = 0.6;
        const MAX_REDUCTION = 0.85;
        const MIN_DEPTH = 10;
        const MAX_DEPTH = 20;

        const depthRange = MAX_DEPTH - MIN_DEPTH;
        const normalizedDepth = (this.maxDepth - MIN_DEPTH) / depthRange;
        this.reduction = MAX_REDUCTION - normalizedDepth * (MAX_REDUCTION - MIN_REDUCTION);

    }

    getZoneColor(xCanvas, type = "leaf") {
        const zoneWidth = this.width / this.departmentZones.length;
        const zoneIndex = Math.floor(xCanvas / zoneWidth);
        const zone = this.departmentZones[Math.min(zoneIndex, this.departmentZones.length - 1)];
        return zone[type];
    }

    fruit(xCanvas) {
        const radius = this.height * 0.01;
        const color = this.getZoneColor(xCanvas, "fruit");
        const gradient = this.ctx.createRadialGradient(0, 0, 0, 0, 0, radius);
        gradient.addColorStop(0, "white");
        gradient.addColorStop(1, color);
        this.ctx.fillStyle = gradient;
        this.ctx.beginPath();
        this.ctx.arc(0, 0, radius, 0, Math.PI * 2);
        this.ctx.fill();
    }

    leaf(xCanvas) {
        const rx = this.height * 0.015;
        const ry = this.height * 0.0075;
        const color = this.getZoneColor(xCanvas, "leaf");
        const gradient = this.ctx.createRadialGradient(0, 0, 0, 0, 0, rx);
        gradient.addColorStop(0, "white");
        gradient.addColorStop(1, color);

        this.ctx.fillStyle = gradient;
        this.ctx.beginPath();
        this.ctx.ellipse(0, 0, rx, ry, 0, 0, 2 * Math.PI);
        this.ctx.fill();

        this.ctx.strokeStyle = color;
        this.ctx.lineWidth = 1;
        this.ctx.beginPath();
        this.ctx.moveTo(-rx / 1.1, 0);
        this.ctx.lineTo(rx / 20, 0);
        this.ctx.stroke();
    }

    branches(len, depth) {
        if (len < this.height * 0.005 || depth > this.maxDepth) {
            const xCanvas = this.ctx.getTransform().e / SCALE_FACTOR;
            if (Math.random() * 100 < this.fruitProb) {
                this.fruit(xCanvas);
            } else {
                this.leaf(xCanvas);
            }
            return;
        }

        for (let dir of [1, -1]) {
            this.ctx.save();
            this.ctx.rotate(this.angle * dir * (Math.random() * 0.4 + 0.8));
            this.ctx.strokeStyle = this.treeGradient;
            this.ctx.lineWidth = len * 0.2;
            this.ctx.beginPath();
            this.ctx.moveTo(0, 0);
            this.ctx.lineTo(len, 0);
            this.ctx.stroke();
            this.ctx.translate(len, 0);
            this.branches(len * this.reduction * (Math.random() * 0.5 + 0.7), depth + 1);
            this.ctx.restore();
        }
    }

    trunk() {
        this.ctx.strokeStyle = this.treeGradient;
        this.ctx.lineWidth = this.len * 0.2;
        this.ctx.beginPath();
        this.ctx.moveTo(0, 0);
        this.ctx.lineTo(this.len, 0);
        this.ctx.stroke();

        this.ctx.save();
        this.ctx.translate(this.len, 0);
        this.branches(this.len * this.reduction, 0);
        this.ctx.restore();
    }

    display(x, y) {
        this.ctx.save();
        this.ctx.translate(x, y);
        this.ctx.rotate(-Math.PI / 2);
        this.trunk();
        this.ctx.restore();
    }
}




let selectedDepartment = "All departments";
const MAX_WIDTH = 1241 / 3;
const MAX_HEIGHT = 1754 / 3;

const departmentColors = {
    "Choisir mon département": {
        leaf: "rgb(12, 136, 6)",
        fruit: "rgb(206, 255, 157)"
    },
    "Interior architecture / Space & Communication": {
        leaf: "rgb(138, 255, 220)",
        fruit: "rgb(255, 138, 138)"
    },
    "Fine Arts": {
        leaf: "rgb(255, 253, 138)",
        fruit: "rgb(255, 138, 208)"
    },
    "Cinema": {
        leaf: "rgb(255, 197, 138)",
        fruit: "rgb(138, 159, 255)"
    },
    "Visual Communication / Media Design": {
        leaf: "rgb(138, 181, 255)",
        fruit: "rgb(138, 255, 220)"
    },
    "Product Design / Fashion & Accessories": {
        leaf: "rgb(255, 138, 138)",
        fruit: "rgb(169, 138, 255)"
    }
};

const SCALE_FACTOR = 10;


const canvas = document.getElementById('fractalTrees');
const ctx = canvas.getContext('2d');


function resizeCanvas() {
    const availableWidth = window.innerWidth;
    const availableHeight = window.innerHeight;

    const displayWidth = Math.min(availableWidth, MAX_WIDTH);
    const displayHeight = Math.min(availableHeight, MAX_HEIGHT);


    canvas.width = displayWidth * SCALE_FACTOR;
    canvas.height = displayHeight * SCALE_FACTOR;


    canvas.style.width = `${displayWidth}px`;
    canvas.style.height = `${displayHeight}px`;

    draw();
}

function draw() {
    ctx.setTransform(1, 0, 0, 1, 0, 0);
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.save();
    ctx.scale(SCALE_FACTOR, SCALE_FACTOR);

    const displayWidth = canvas.width / SCALE_FACTOR;
    const displayHeight = canvas.height / SCALE_FACTOR;

    const gradient = ctx.createLinearGradient(0, 0, 0, displayHeight);
    gradient.addColorStop(0, "#ffffff");
    gradient.addColorStop(1, "rgb(0, 13, 255)");
    ctx.fillStyle = gradient;
    ctx.fillRect(0, 0, displayWidth, displayHeight);


    const maxDepth = PROMOTION_RECURSION_DEPTHS[selectedPromotion] || 5;

    if (selectedDepartment === "All departments") {
        const departmentList = [
            "Interior architecture / Space & Communication",
            "Fine Arts",
            "Cinema",
            "Visual Communication / Media Design",
            "Product Design / Fashion & Accessories"
        ];

        const zones = departmentList.map(dep => ({
            leaf: departmentColors[dep].leaf,
            fruit: departmentColors[dep].fruit
        }));

        const tree = new Tree(ctx, displayWidth, displayHeight, "rgb(61, 94, 60)", zones, maxDepth);
        tree.display(displayWidth / 2, displayHeight);
    } else {
        const colors = departmentColors[selectedDepartment] || departmentColors["Choisir mon département"];
        const tree = new Tree(ctx, displayWidth, displayHeight, "rgb(61, 94, 60)", [colors], maxDepth);
        tree.display(displayWidth / 2, displayHeight);
    }

    ctx.restore();
}



const PROMOTION_RECURSION_DEPTHS = {
    2009: 10,/*level*/
    2010: 10.2,/*level*/
    2011: 10.4,
    2012: 10.6,/*level*/
    2013: 10.8,
    2014: 11,/*level*/
    2015: 11.3,
    2016: 11.5,/*level*/
    2017: 11.7,
    2018: 12,/*level*/
    2019: 12.3,
    2020: 12.5,/*level*/
    2021: 12.7,
    2022: 13,/*level*/
    2023: 13.3,
    2024: 13.5,/*level*/
    2025: 14 /*level*/
};



window.addEventListener('resize', resizeCanvas);
resizeCanvas();
