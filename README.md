# FoldingCell

[shot on dribbble](https://dribbble.com/shots/2121350-Delivery-Card):
![Animation](Screenshots/folding-cell.gif)

## Requirements

- iOS 8.0+
- Xcode 7.2

## Installation

Just add the FoldingCell.swift file to your project.

or use [CocoaPods](https://cocoapods.org) with Podfile:
``` ruby
pod 'FoldingCell', '~> 0.3'
```
    

## Solution
![Solution](/Tutorial-resources/Solution.png)
## Usage

1) Create a new cell inheriting from `FoldingCell`

2) Add a UIView to your cell in your storyboard or nib file, inheriting from `RotatedView`.
Connect the outlet from this view to the cell property `foregroundView`.
Add constraints from this view to the superview, as in this picture: 
![1.1](/Tutorial-resources/1.1.png)

(constants of constraints may be different). Add the identifier `ForegroundViewTop`
for the top constraint. (This view will be shown when the cell is in its normal state).

3) Add other UIViews to your cell, connect the outlet from this view to the cell
property `containerView`. Add constraints from this view to the superview like in the picture:

![1.2](/Tutorial-resources/1.2.png)

(constants of constraints may be different). Add the identifier "ContainerViewTop" for the top constraint.
(This view will be shown when the cell is opened)

Your result should be something like this picture:
![1.3](/Tutorial-resources/1.3.png)

4) Set ``` @IBInspectable var itemCount: NSInteger ``` property is a count of folding (it IBInspectable you can set in storyborad). range 2 or greater. Default value is 2

Ok, we've finished configuring the cell.

5) Adding code to your UITableViewController

5.1) Add constants:
``` swift
     let kCloseCellHeight: CGFloat = *** // equal or greater foregroundView height
     let kOpenCellHeight: CGFloat = *** // equal or greater containerView height
```
5.2) Add property

``` swift
     var cellHeights = [CGFloat]()
```

     create in viewDidLoad:
``` swift
     override func viewDidLoad() {
        super.viewDidLoad()

        for _ in 0...kRowsCount {
            cellHeights.append(kCloseCellHeight)
        }
    }
```

5.3) Override method:
``` swift
    override func tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat {
        return cellHeights[indexPath.row]
    }
```

5.4) Added code to method:
``` swift
    override func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        let cell = tableView.cellForRowAtIndexPath(indexPath) as! FoldingCell

        var duration = 0.0
        if cellHeights[indexPath.row] == kCloseCellHeight { // open cell
            cellHeights[indexPath.row] = kOpenCellHeight
            cell.selectedAnimation(true, animated: true, completion: nil)
            duration = 0.5
        } else {// close cell
            cellHeights[indexPath.row] = kCloseCellHeight
            cell.selectedAnimation(false, animated: true, completion: nil)
            duration = 1.1
        }

        UIView.animateWithDuration(duration, delay: 0, options: .CurveEaseOut, animations: { () -> Void in
            tableView.beginUpdates()
            tableView.endUpdates()
        }, completion: nil)
    }
```
5.5) Control if the cell is open or closed
``` swift
  override func tableView(tableView: UITableView, willDisplayCell cell: UITableViewCell, forRowAtIndexPath indexPath: NSIndexPath) {

        if cell is FoldingCell {
            let foldingCell = cell as! FoldingCell

            if cellHeights![indexPath.row] == kCloseCellHeight {
                foldingCell.selectedAnimation(false, animated: false, completion:nil)
            } else {
                foldingCell.selectedAnimation(true, animated: false, completion: nil)
            }
        }
    }
``` 

6) Add this code to your new cell class
``` swift
    override func animationDuration(itemIndex:NSInteger, type:AnimationType)-> NSTimeInterval {

        // durations count equal it itemCount
        let durations = [0.33, 0.26, 0.26] // timing animation for each view
        return durations[itemIndex]
    }
```

## [About Us](http://ramotion.com?utm_source=gthb&utm_medium=special&utm_campaign=folding-cell)

Designed & Developed in [Ramotion - Digital Design Agency](http://ramotion.com?utm_source=gthb&utm_medium=special&utm_campaign=folding-cell)  
We are ready for new interesting iOS app development projects.

Follow us on [Twitter](http://twitter.com/ramotion).
