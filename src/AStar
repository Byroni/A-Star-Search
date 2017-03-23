import java.io.IOException;
import java.util.*;

/**
 * Created by byron on 3/22/17.
 */
public class AStar {

    /* PSEUDO

    generate 15x15 board with 10% blocked

    initialize open list    >
    initialize closed list  >
    put the starting node on the open list with f = 0

    while open list is not empty
        find the node with the least f on the open list, call it 'q'
        pop 'q' off open list
        generate q's 8 successors and set their parents to 'q'

        for each successor
            if successor is the goal, stop the search
            successor.g = q.g + distance between successor and q
            successor.h = distance from goal to successor
            successor.f = successor.g + successor.h

            if node with the same position as successor is in the open list
                && has a lower f than successor, skip this successor
            if node with the same position as successor is in the closed list
                $$ has a lower f than successor, skip this successor
            otherwise, add the node to the open list
        end
        push q to closed list
    end

     */

    //  Console text colors
    public static final String ANSI_RESET = "\u001B[0m";
    public static final String ANSI_BLACK = "\u001B[30m";
    public static final String ANSI_RED = "\u001B[31m";
    public static final String ANSI_GREEN = "\u001B[32m";
    public static final String ANSI_YELLOW = "\u001B[33m";
    public static final String ANSI_BLUE = "\u001B[34m";
    public static final String ANSI_PURPLE = "\u001B[35m";
    public static final String ANSI_CYAN = "\u001B[36m";
    public static final String ANSI_WHITE = "\u001B[37m";

    private static List<Node> open = new ArrayList<>();
    private static List<Node> closed = new ArrayList<>();
    private static Node goalNode;
    private static int nodeCost = 10;
    private static int[][] blockers;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Node startNode;
        blockers = generateBlockers().clone();

//        System.out.print("Start X: ");
//        int sx= sc.nextInt();
//        System.out.print("Start Y: ");
//        int sy = sc.nextInt();
//        System.out.print("End X: ");
//        int ex = sc.nextInt();
//        System.out.print("End Y: ");
//        int ey = sc.nextInt();
//
//        if ((Math.abs(sx-ex) + Math.abs(sy-ey))/2 >= 10) {
//            System.out.println("----------------------------------------------\nWARNING... any coordinates 10 or more diagonal\nnodes apart will take a long time to calculate\n----------------------------------------------");
//        }
//        //  Set goalNode
//        goalNode = new Node(ey, ex, 1);
//        startNode = new Node(sy, sx, 1);
//
//        //  Add starting node to open list
//        open.add(startNode);
//
        goalNode = new Node(0, 0, 1);
        startNode = new Node(9, 9, 1);
        open.add(startNode);
        open.get(0).setG(0);
        open.get(0).setH(0);
        open.get(0).setF();

        printBoard(startNode, goalNode,null, null);

        while (!open.isEmpty()) {
            checkNext();
        }
    }

    public static void checkNext() {
        Node bestNode = null;
        int min = 1000;

        for (Node node : open) {
            if (node.getF() <= min) {
                bestNode = node;
                min = node.getF();
            }
        }
        Node nextNode = open.get(open.indexOf(bestNode));
        open.remove(open.indexOf(bestNode));

        expandNode(nextNode);
    }

    public static void expandNode(Node node) {
        List<Node> children = new ArrayList<>();

        for (int i = 0; i < 3; i++) {
            for(int j = 0; j < 3; j++) {
                if (i == 1 && j == 1) {
                } else if (node.getRow()-1+i < 0 || node.getRow()-1+i > 14 || node.getCol()-1+j < 0 || node.getCol()-1+j > 14) {
                } else
                    children.add(new Node(node.getRow()-1+i, node.getCol()-1+j, 1));
            }
        }
        for (Node child : children) {
            //  Check is goal node
            if (child.getCol() == goalNode.getCol() && child.getRow() == goalNode.getRow()) {
//                System.out.println("Found goal node with: " + child);
                child.setParent(node);
                closed.add(child);
                printPath();
                System.exit(0);
            } else {
                child.setG(node.getG() + nodeCost);
                //  Manhattan distance calculation
                child.setH(Math.abs(goalNode.getCol() - child.getCol()) + Math.abs(goalNode.getRow() - child.getRow()));
                child.setF();
                child.setParent(node);
            }

            if (checkLists(child)) {
                open.add(child);
            }
        }
        closed.add(node);
    }

    public static boolean checkLists(Node child) {
        //  check if child is in the open or closed lists
        //  if node has lower f, return true, else false

        //  check open list
        for (Node node : open) {
            if (child.getCol() == node.getCol() && child.getRow() == node.getRow()) {
                if (child.getF() > node.getF())
                    return false;
            }
        }
        //  check closed list
        for (Node node : closed) {
            if (child.getCol() == node.getCol() && child.getRow() == node.getRow()) {
                if (child.getF() > node.getF())
                    return false;
            }
        }
        return true;
    }

    public static void printPath () {
        int split = 0;
        Node parent = closed.get(closed.size()-1);

        System.out.print("\nPATH: ");
        while (parent != null) {
            if (split%3 == 0) {
                System.out.println();
            }
            System.out.print(parent + " -> ");
            parent = parent.getParent();
            ++split;
        }
    }

    public static void printBoard (Node start, Node end, List<Node> open, List<Node> closed) {
        int[][] board = new int[15][15];
        // j = x
        // i = y

        System.out.println("\n-----------------------------------------");
        System.out.println(ANSI_GREEN + "Start" + ANSI_PURPLE + "\tGoal" + ANSI_BLUE + "\tOpen" + ANSI_YELLOW + "\tClosed" + ANSI_RED + "\t Blocked" + ANSI_RESET);
        System.out.println("-----------------------------------------");

        for (int i = 0; i < 15; i++) {
            for (int j = 0; j < 15; j++) {
                if (i == start.getRow() && j == start.getCol())     //  Place start
                    System.out.print(ANSI_GREEN + "S " + ANSI_RESET);
                else if (i == end.getRow() && j == end.getCol())    //  Place goal
                    System.out.print(ANSI_PURPLE + "G " + ANSI_RESET);
                else if (checkBlockers(j, i))
                    System.out.print(ANSI_RED + "B " + ANSI_RESET);
                else
                    System.out.print(board[i][j] + " ");
            }
            System.out.println();
        }
    }

    //  Randomly block 10% of board
    public static int[][] generateBlockers () {
        int[][] blockers;

        blockers = new int[15][15];

        Random rd = new Random();

        for (int i = 0; i < 23; i++) {  //  23 = (15^2)*.1
            int x = rd.nextInt(15);
            int y = rd.nextInt(15);

            //  If the node is already blocked
            if (blockers[x][y] == 1) {
                blockers[rd.nextInt(15)][rd.nextInt(15)] = 1;   //  Try another node
                i++;
            } else  //  Otherwise block it
                blockers[x][y] = 1;
        }

        return blockers;
    }

    public static boolean checkBlockers (int x, int y) {
        if (blockers[y][x] == 1)
            return true;
        return false;
    }
}
