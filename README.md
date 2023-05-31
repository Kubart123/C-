using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace RPGGame
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Welcome to the RPG Game!");

            // Get player's name
            Console.Write("Enter your name: ");
            string playerName = Console.ReadLine();

            // Create a player
            Player player = new Player(playerName);

            // Create an enemy
            Enemy enemy = new Enemy("Goblin");

            // Game loop
            while (player.IsAlive && enemy.IsAlive)
            {
                Console.WriteLine("\nPlayer's turn:");
                Console.WriteLine("1. Attack");
                Console.WriteLine("2. Heal");

                // Get player's choice
                int choice;
                while (!int.TryParse(Console.ReadLine(), out choice) || choice < 1 || choice > 2)
                {
                    Console.WriteLine("Invalid input! Please enter 1 or 2.");
                }

                if (choice == 1)
                {
                    // Player attacks enemy
                    player.Attack(enemy);
                }
                else
                {
                    // Player heals themselves
                    player.Heal();
                }

                // Check if enemy is defeated
                if (!enemy.IsAlive)
                {
                    Console.WriteLine("Congratulations! You defeated the enemy!");
                    break;
                }

                // Enemy's turn
                Console.WriteLine("\nEnemy's turn:");
                enemy.Attack(player);

                // Check if player is defeated
                if (!player.IsAlive)
                {
                    Console.WriteLine("Game Over! You were defeated by the enemy.");
                    break;
                }
            }

            Console.WriteLine("\nThank you for playing the RPG Game!");
        }
    }

    class Player
    {
        public string Name { get; }
        public int Health { get; private set; }
        public bool IsAlive => Health > 0;

        public Player(string name)
        {
            Name = name;
            Health = 100;
        }

        public void Attack(Enemy enemy)
        {
            Random random = new Random();
            int damage = random.Next(10, 21); // Generate a random damage between 10 and 20
            enemy.TakeDamage(damage);
            Console.WriteLine($"You attacked the enemy and dealt {damage} damage.");
        }

        public void Heal()
        {
            Random random = new Random();
            int heal = random.Next(10, 21); // Generate a random heal between 10 and 20
            Health += heal;
            Console.WriteLine($"You healed yourself and gained {heal} health.");
        }

        public void TakeDamage(int damage)
        {
            Health -= damage;
            if (Health < 0)
                Health = 0;
        }
    }

    class Enemy
    {
        public string Name { get; }
        public int Health { get; private set; }
        public bool IsAlive => Health > 0;

        public Enemy(string name)
        {
            Name = name;
            Health = 50;
        }

        public void Attack(Player player)
        {
            Random random = new Random();
            int damage = random.Next(5, 16); // Generate a random damage between 5 and 15
            player.TakeDamage(damage);
            Console.WriteLine($"The enemy attacked you and dealt {damage} damage.");
        }

        public void TakeDamage(int damage)
        {
            Health -= damage;
            if (Health < 0)
                Health = 0;
        }
    }
}
