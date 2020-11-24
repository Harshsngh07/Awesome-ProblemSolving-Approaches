Sample code which can solve all below problem.

https://leetcode.com/problems/binary-tree-level-order-traversal/
https://leetcode.com/problems/binary-tree-level-order-traversal-ii/
https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
https://leetcode.com/problems/average-of-levels-in-binary-tree/

https://leetcode.com/problems/binary-tree-level-order-traversal/

```
ArrayList<List<Integer>>  result=new ArrayList<List<Integer>>();
   public List<List<Integer>> levelOrder2(TreeNode root) {
       if(root==null) return result;
       levelOrder2(root,0);
       return result;

   }

private void  levelOrder2(TreeNode node,int level){
       if(result.size()==level)
           result.add(new ArrayList<Integer>());
       result.get(level).add(node.val);
       if(node.left!=null)
           levelOrder2(node.left,level+1);
       if(node.right!=null)
           levelOrder2(node.right,level+1);
   }

```

https://leetcode.com/problems/binary-tree-level-order-traversal-ii/

    private void  levelOrderBottom(TreeNode node,int level){
       if(result.size()==level)
           result.add(0,new ArrayList<Integer>());
       if(node.left!=null)
           levelOrderBottom(node.left,level+1);
       if(node.right!=null)
           levelOrderBottom(node.right,level+1);
       result.get(result.size()-level-1).add(node.val);
   }
https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/


    private void  zigzagLevelOrder(TreeNode node,int level){
        if(result.size()==level)
            result.add(new ArrayList<Integer>());
        if(level%2==0)
            result.get(level).add(node.val);
        else
            result.get(level).add(0,node.val);
        if(node.left!=null)
            zigzagLevelOrder(node.left,level+1);
        if(node.right!=null)
            zigzagLevelOrder(node.right,level+1);
    }
https://leetcode.com/problems/average-of-levels-in-binary-tree/

    public List<Double> averageOfLevels(TreeNode root) {
        ArrayList<Double>  value=new ArrayList<>();
         if(root==null) return value;
        averageOfLevels(root,0);
        for(List<Integer> res :result)
        {
           value.add(res.stream().mapToInt(a -> a).average().orElse(0.0));
        }
        return value;

    }
    private void  averageOfLevels(TreeNode node,int level){
        if(result.size()==level)
            result.add(new ArrayList<Integer>());
        result.get(level).add(node.val);
        if(node.left!=null)
            averageOfLevels(node.left,level+1);
        if(node.right!=null)
            averageOfLevels(node.right,level+1);
    } 

<hr/>    

While there are many posts shareing different implementations with tree traverses, it is a bit cumbersome to memorize different implementations for different traversal types. And it requires lots of effort to ensure a bug-free implementation.

For better interview purpose, I would like to share my template that works uniformly for pre-order, in-order and post-order traversals. It is easily memorizable, and you can simply write it down in the whiteboard on spot.

The idea is still to use a Stack to keep the traverse path. The trick in the template is to keep track of the occurances of a node. Depending the occurance, we know if it is ready to be popped for post-order, in-order, and pre-order. Below is the template:

    @AllArgsConstructor
    class Pair {
        TreeNode treeNode;
        int visited;
     }

    public void treeTraverse(TreeNode root) {
        Deque<Pair> stack = new LinkedList<>();
        stack.push(new Pair(root, 0));

        while (!stack.isEmpty()) {
            Pair top = stack.pop();
            if (top.treeNode == null) {
                continue;
            }
			if(top.visited == 2) {
				System.out.println("Both children has been processed");
			} else if(top.visited == 1) {
			   System.out.println("Left children has been processed");
			   top.visited++;
			   stack.push(node);
			   //do something
			} else {
              System.out.println("No children has been processed");
			   top.visited++;
			   stack.push(node);
			   //do something
			}
        }
    }
So this template push a node at most three times, and based on its occurance, we can determine the state of the traverse.
The simple rule is:

visited == 0, no children has been processed, if we print current node, it is pre-order traversal
visited == 1, all left children has been processed, if we print current node, it is in-order traversal
visited == 2, all left and right children has been processed, if we print current node, it is post-order traversal.
The completed AC code for the three variants are as below. Although they are not the fastest implementation, but they are easy to remember and bug-free.

Post-order:
```
public List<Integer> postorderTraversal(TreeNode root) {
        Deque<Pair> stack = new LinkedList<>();
        stack.push(new Pair(root, 0));
        List<Integer> ans = new LinkedList<>();
        while (!stack.isEmpty()) {
            Pair top = stack.pop();
            if (top.treeNode == null) {
                continue;
            }
            if (top.visited == 2) {
                ans.add(top.treeNode.val);
            } else if (top.visited == 1) {
                top.visited++;
                stack.push(top); 
                stack.push(new Pair(top.treeNode.right, 0));
            } else {
                top.visited++;
                stack.push(top); 
                stack.push(new Pair(top.treeNode.left, 0));
            }
        }
        return ans;
    }

```    
In-order:
```
 public List<Integer> inorderTraversal(TreeNode root) {
        Deque<Pair> stack = new LinkedList<>();
        stack.push(new Pair(root, 0));
        List<Integer> ans = new LinkedList<>();
        while (!stack.isEmpty()) {
            Pair top = stack.pop();
            if (top.treeNode == null) {
                continue;
            }
            if (top.visited == 1) {
                ans.add(top.treeNode.val);
				stack.push(new Pair(top.treeNode.right, 0));
            } else {
                top.visited++;
                stack.push(top);
				stack.push(new Pair(top.treeNode.left, 0));
            }
        }
        return ans;
    }

```    
Pre-order:
```
public List<Integer> preorderTraversal(TreeNode root) {
        Deque<Pair> stack = new LinkedList<>();
        stack.push(new Pair(root, 0));
        List<Integer> ans = new LinkedList<>();
        while (!stack.isEmpty()) {
            Pair top = stack.pop();
            if (top.treeNode == null) {
                continue;
            }
            if(top.visited == 0) { // it is not necessary for pre-order to have this check, but here is for the integrity of template
                ans.add(top.treeNode.val);
				stack.push(new Pair(top.treeNode.right, 0));
				stack.push(new Pair(top.treeNode.left, 0));
            }
        }
        return ans;
    }
```    
Hope this would help you during the coding interview.

P.S. In the above template, we only use one stack. Another benefit it can bring is to implement tree-iterators easily. The DFS solution in the reference solution does not provide such benefit.