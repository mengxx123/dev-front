<template>
    <my-page title="图（数据结构）" :page="page">
        <div class="common-container">
            <div class="canvasBox">
                <canvas class="canvas" id="canvas"
                    @touchstart="onTouchStart($event)"
                    @touchmove="onTouchMove($event)"
                    @touchend="onTouchEnd($event)"
                    @mousedown="onMouseDown($event)"
                    @mousemove="onMouseMove($event)"
                    @mouseup="onMouseUp($event)"
                ></canvas>
                <!-- <div class="result">{{ result }}</div> -->
            </div>
            <div class="stat">
                顶点：{{ points.length }}，边：{{ lines.length }}
            </div>
        </div>
    </my-page>
</template>

<script>
    /* eslint-disable */
    function getAngle(x1, y1, x2, y2) {
        // 直角的边长
        var x = Math.abs(x1 - x2);
        var y = Math.abs(y1 - y2);
        // 斜边长
        var z = Math.sqrt(Math.pow(x, 2) + Math.pow(y, 2));
        // 余弦
        var cos = y / z;
        // 弧度
        var radina = Math.acos(cos);
        // 角度
        var angle = 180 / (Math.PI / radina);
        return angle;
    }

    function getDegAngle(x, y) {
        let hypotenuse = Math.sqrt(Math.pow(x, 2)+Math.pow(y, 2));
        //斜边长度
        let cos = x/hypotenuse;
        let radian = Math.acos(cos);
        //求出弧度
        let angle = 180/(Math.PI/radian);
        //用弧度算出角度
        if (y < 0) {
            angle = -angle;
        } else if ((y == 0) && (x < 0)) {
            angle = 180;
        }
        if (x > 0) {
            if (y > 0) {
                // 1
                // console.log('=1')
                return 90 - angle
            } else {
                // 2
                // console.log('=2')
                return 90 - angle
            }
        } else {
            if (y < 0) {
                // 3
                // console.log('=3')
                return 90 - angle
            } else {
                // 4
                // console.log('=4')
                return 360 + (90 - angle)
            }
        }
    }

    function getLine(a, b) {
        let A = b.y - a.y
        let B = a.x - b.x
        let C = b.x * a.y - a.x * b.y
        return {
            A,
            B,
            C,
        }
    }
    function getY(line, x) {
        return (0 - line.A * x - line.C) / line.B
    }

    console.log('getDegAngle', getDegAngle(0, -100))

    export default {
        data () {
            return {
                points: [
                    // {
                    //     x: 10,
                    //     y: 10,
                    // },
                    // {
                    //     x: 100,
                    //     y: 10,
                    // },
                    // {
                    //     x: 10,
                    //     y: 100,
                    // },
                ],
                lines: [
                    // {
                    //     from: 0,
                    //     to: 1,
                    // }
                ],
                result: '0.00',
                src: '',
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            href: 'https://project.yunser.com/products/542067d0429d11ea98f08b5ebbd167fa',
                            target: '_blank'
                        }
                    ]
                }
            }
        },
        mounted() {
            this.init()
            this.debug()

            // function pathsToTree(fileList) {
            //     let result = {
            //         name: '_root',
            //         children: []
            //     }
            //     for (let path of fileList) {
            //         console.log('path', path)
            //         if (path.includes('/')) { // 文件夹
            //             let paths = path.split('/')
            //             let currentPath = result
                        
            //             for (let i = 0; i < paths.length; i++) {
            //                 let p = paths[i]
            //                 console.log('item', p)
            //                 let findItem = currentPath.children.find(item => item.name === p)
            //                 if (findItem) {
            //                     currentPath = findItem
            //                 } else {
            //                     let isFile = i === paths.length - 1
            //                     let folder = {
            //                         type: isFile ? 'file' : 'folder',
            //                         name: p,
            //                     }
            //                     if (!isFile) {
            //                         folder.children = []
            //                     }
            //                     currentPath.children.push(folder)
            //                     currentPath = folder
            //                 }
            //             }
            //         } else { // 文件
            //             result.children.push({
            //                 type: 'file',
            //                 name: path
            //             })
            //         }
            //     }
            //     return result.children
            // }
            // console.log(pathsToTree([
            //     '1.txt',
            //     '2.txt',
            //     'test/1.txt',
            //     'test/2.txt',
            //     'test/asd/1.txt',
            //     'test2/1.txt',
            // ])
// )
        },
        methods: {
            debug() {
                // this.points = [
                //     {
                //         x: 200,
                //         y: 300,
                //     },
                //     {
                //         x: 300,
                //         y: 200,
                //     },
                //     {
                //         x: 300,
                //         y: 400,
                //     },
                //     {
                //         x: 400,
                //         y: 400,
                //     },
                // ]
                this.reDraw()
            },
            init() {
                this.canvas = document.getElementById('canvas')
                let rect = this.canvas.getBoundingClientRect()
                this.canvasWidth = rect.width
                this.canvasHeight = rect.height

                this.canvas.style.width = this.canvasWidth + 'px'
                this.canvas.style.height = this.canvasHeight + 'px'
                this.canvas.setAttribute('width', this.canvasWidth + 'px')
                this.canvas.setAttribute('height', this.canvasHeight + 'px')
                
                
                let ctx = this.canvas.getContext('2d')
                ctx.width = rect.width
                ctx.height = rect.height
                this.ctx = ctx

                this.reDraw(ctx)
            },
            onTouchStart(e) {
                e.preventDefault()
                this.onMouseDown(e.targetTouches[0])
                this.latestPt = e.targetTouches[0]
            },
            onTouchMove(e) {
                e.preventDefault()
                this.onMouseMove(e.targetTouches[0])
                this.latestPt = e.targetTouches[0]
            },
            onTouchEnd(e) {
                e.preventDefault()
                this.onMouseUp()
            },
            onMouseDown(e) {
                this.latestPt = e
                
                this.downIndex = -1
                this.isDown = true
                let rect = document.getElementById('canvas').getBoundingClientRect()
                let x = e.pageX - rect.left
                let y = e.pageY - rect.top
                this.downPt = {
                    x,
                    y,
                }

                // this.points.push({
                //     x,
                //     y,
                // })
                this.reDraw()
                // return

                function getDistance(a, b) {
                    return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2) + ((a.z || b.z) ? Math.pow(a.z - b.z, 2) : 0))
                }

                for (let i = 0; i < this.points.length; i++) {
                    let pt = this.points[i]
                    if (getDistance(this.downPt, this.points[i]) < 40) {
                        console.log('is', i)
                        this.downIndex = i
                        this.originX = pt.x
                        this.originY = pt.y
                        break
                    }
                }

                // if (this.downIndex === -1) {
                //     let line1 = getLine(this.points[1], this.points[0])
                //     let y1 = getY(line1, x)
                //     if (Math.abs(y1 - y) < 40) {
                //         console.log('是线')
                //         let d1 = getDistance(this.downPt, this.points[0])
                //         let d2 = getDistance(this.downPt, this.points[1])
                //         if (d1 < d2) {
                //             console.log('左边进')
                //             this.downIndex = 0
                //             this.originX = this.points[0].x
                //             this.originY = this.points[0].y
                //         } else {
                //             console.log('右边进')
                //             this.downIndex = 1
                //             this.originX = this.points[this.downIndex].x
                //             this.originY = this.points[this.downIndex].y
                //         }
                //         return
                //     }

                //     let line2 = getLine(this.points[3], this.points[2])
                //     let y2 = getY(line2, x)
                //     if (Math.abs(y2 - y) < 40) {
                //         console.log('是线')
                //         let d1 = getDistance(this.downPt, this.points[2])
                //         let d2 = getDistance(this.downPt, this.points[3])
                //         if (d1 < d2) {
                //             console.log('左边进')
                //             this.downIndex = 2
                //             this.originX = this.points[this.downIndex].x
                //             this.originY = this.points[this.downIndex].y
                //         } else {
                //             console.log('右边进')
                //             this.downIndex = 3
                //             this.originX = this.points[this.downIndex].x
                //             this.originY = this.points[this.downIndex].y
                //         }
                //         return
                //     }

                // }
            },
            onMouseMove(e) {
                if (!this.isDown) {
                    return
                }
                if (this.downIndex === -1) {
                    return
                }
                let rect = document.getElementById('canvas').getBoundingClientRect()
                let x = e.pageX - rect.left
                let y = e.pageY - rect.top
                this.latestPt = e
                this.tmpLine = [
                    this.downPt,
                    {
                        x,
                        y,
                    }
                ]
                console.log(x, y)
                // let offsetX = x - this.downPt.x
                // let offsetY = y - this.downPt.Y

                // this.points[this.downIndex].x = x
                // this.points[this.downIndex].y = y
                this.reDraw()
            },
            onMouseUp(e) {
                function getDistance(a, b) {
                    return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2) + ((a.z || b.z) ? Math.pow(a.z - b.z, 2) : 0))
                }
                let rect = document.getElementById('canvas').getBoundingClientRect()
                let x = this.latestPt.pageX - rect.left
                let y = this.latestPt.pageY - rect.top


                this.isDown = false
                if (getDistance(this.downPt, {
                    x,
                    y,
                }) < 10) {
                    this.points.push({
                        x,
                        y,
                    })
                    this.reDraw()
                    return
                }
                if (this.downIndex === -1) {
                    return
                }
                
                this.downPt = {
                    x,
                    y,
                }

                
                // return

                
                this.upIndex = -1
                for (let i = 0; i < this.points.length; i++) {
                    let pt = this.points[i]
                    if (getDistance(this.downPt, this.points[i]) < 40) {
                        console.log('is', i)
                        this.upIndex = i
                        break
                    }
                }
                if (this.upIndex !== -1) {
                    console.log('多了一条线')
                    this.lines.push({
                        from: this.downIndex,
                        to: this.upIndex,
                    })
                    // return
                }

                this.tmpLine = null


                this.reDraw()
            },
            reDraw() {
                // ctx.fillRect(0, 0, 50, 50, 0, 0, 20, 20)

                this.ctx.clearRect(0, 0, this.canvasWidth, this.canvasHeight)
                this.ctx.lineWidth = 2
                for (let item of this.points) {
                    console.log('item', item)
                    this.ctx.beginPath()
                    this.ctx.fillStyle = '#000'
                    this.ctx.arc(item.x, item.y, 8, 0, Math.PI * 2)
                    this.ctx.fill()
                }

                for (let item of this.lines) {
                    this.ctx.setLineDash([])
                    let fromPt = this.points[item.from]
                    let toPt = this.points[item.to]
                    this.ctx.beginPath()
                    this.ctx.moveTo(fromPt.x, fromPt.y)
                    this.ctx.lineTo(toPt.x, toPt.y)
                    this.ctx.strokeStyle = '#000'
                    this.ctx.stroke()
                }
                if (this.tmpLine) {
                    this.ctx.setLineDash([10, 8])
                    this.ctx.beginPath()
                    this.ctx.moveTo(this.tmpLine[0].x, this.tmpLine[0].y)
                    this.ctx.lineTo(this.tmpLine[1].x, this.tmpLine[1].y)
                    this.ctx.strokeStyle = '#000'
                    this.ctx.stroke()
                }
            },
        }
    }
</script>

<style lang="scss" scoped>

.canvasBox {
    // position: fixed;
    // top: 64px;
    // left: 0;
    // right: 0;
    // bottom: 0;
    // border: 1px solid #000;
    // background: #fff;
    // z-index: 1000000;
    .result {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        text-align: center;
        font-size: 24px;
        padding-top: 16px;
    }
    .canvas {
        width: 100%;
        height: 400px;
        border: 1px solid rgba(0, 0, 0, .12);
        cursor: crosshair;
        user-select: none;
    }
}

</style>
