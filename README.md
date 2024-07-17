# delete-nodes-and-return-forest
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        Set<Integer> toDeleteSet = new HashSet<>();
        for (int val : to_delete) {
            toDeleteSet.add(val);
        }

        List<TreeNode> result = new ArrayList<>();
        if (root != null && !toDeleteSet.contains(root.val)) {
            result.add(root);
        }

        dfs(root, toDeleteSet, result);
        return result;
    }

    private TreeNode dfs(TreeNode node, Set<Integer> toDeleteSet, List<TreeNode> result) {
        if (node == null) {
            return null;
        }

        node.left = dfs(node.left, toDeleteSet, result);
        node.right = dfs(node.right, toDeleteSet, result);

        if (toDeleteSet.contains(node.val)) {
            if (node.left != null) {
                result.add(node.left);
            }
            if (node.right != null) {
                result.add(node.right);
            }
            return null;
        }

        return node;
    }

    public static void main(String[] args) {
        // Example usage:
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.left = new TreeNode(6);
        root.right.right = new TreeNode(7);

        int[] to_delete = {3, 5};

        Solution sol = new Solution();
        List<TreeNode> forest = sol.delNodes(root, to_delete);

        for (TreeNode tree : forest) {
            printTree(tree);
            System.out.println();
        }
    }

    private static void printTree(TreeNode root) {
        if (root == null) {
            return;
        }
        System.out.print(root.val + " ");
        printTree(root.left);
        printTree(root.right);
    }
}
