---
title: "Drag and Drop from a TreeView to a DataGridView"
date: "2011-04-29T14:56:00"
tags: [
  "development"
]
---
[![image](image_thumb.png "image")](https://kapie.com/content/binary/Windows-Live-Writer/Drag-and-Drop-from-a-TreeView-to-a-DataG_D109/image_2.png)An application I was working on recently needed the ability for the user to drag a TreeView node onto a DataGridView, and when ‘dropped’ create a new row in the DataGridView.[![image](image_thumb_1.png "image")](https://kapie.com/content/binary/Windows-Live-Writer/Drag-and-Drop-from-a-TreeView-to-a-DataG_D109/image_4.png)

It is not difficult, but took me a while to put all the relevant pieces together, so I thought I’d post it in case someone else finds it useful.

[![image](image_thumb_2.png "image")](https://kapie.com/content/binary/Windows-Live-Writer/Drag-and-Drop-from-a-TreeView-to-a-DataG_D109/image_6.png)

There are four key aspects to it, summarized as :

1.  Call ‘DoDragDrop’ in the ‘ItemDrag’ event of the source control.
2.  Setting the ‘DragDropEffect’ in the ‘DragEnter’ event of the destination control
3.  Setting ‘AllowDrop’ to true on the destination control
4.  Handling the ‘DragDrop’ event on the destination control.

Inside the ‘ItemDrag’ event we call the DoDragDrop function, this initiates the whole drag drop feature. Setting the DragDropEffect in the DragEnter event on the destination control gives the user a visual clue as to whether it is a move, copy, shortcut etc, by changing the pointer to the correct icon.  
Setting ‘AllowDrop’ to true enables the destination control to ‘accept’ the item being dropped onto it, and finally in the DragDrop event of the destination control we do the actual work (get the details of the item that was dragged and put it into the DataGridView in our case).

Another thing to consider is whether you just want to add the dragged item to the destination control, or whether you want to insert it at a particular location based on where the item was dropped.  
To insert at a particular location, you’ll need to get the X and Y co-ords of the drop location, then translate this to the correct X and Y location with reference to the control itself, and from that get the current item that lives at that location on the control and use that as the reference for inserting the new item.

The code below shows the relevant bits :

void treeView1\_ItemDrag(object sender, System.Windows.Forms.ItemDragEventArgs e)
{
    DoDragDrop(e.Item, DragDropEffects.Copy);
}

void dgvSteps\_DragEnter(object sender, System.Windows.Forms.DragEventArgs e)
{
    e.Effect = DragDropEffects.Copy;
}

private void dgvSteps\_DragDrop(object sender, DragEventArgs e)
{
    TreeNode treeNode = (TreeNode)e.Data.GetData(typeof(TreeNode));
    if (treeNode.Tag == null)
    {
        return; // ignore the category nodes
    }

    // Find out where the drop occurred
    Point pnt = dgvSteps.PointToClient(new Point(e.X, e.Y));
    DataGridView.HitTestInfo info = dgvSteps.HitTest(pnt.X, pnt.Y);
    if (info.RowIndex == –1)
    {
        // it was dropped on an empty area so add at end
        InsertNewRowAtEnd((MyObj)treeView.Tag);
    }
    else
    {
        // it was dropped on an existing row, so insert before that row
        InsertNewRow((MyObj)treeView.Tag, info.RowIndex);
    }
    RefreshDataView();
}

Hope someone finds this useful. Drop me a line in the comments with any questions, or if you found this useful.

GEO: 51.4043807983398 : \-1.2872029542923