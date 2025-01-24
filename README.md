# ShoppingCart with Object
using System;
using System.Windows.Forms;

namespace pj
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        // Class for menu items
        public class MenuItem
        {
            public string Name { get; set; }
            public double Price { get; set; }
            public int Quantity { get; set; }
            public bool Selected { get; set; }

            public double TotalPrice()
            {
                return Selected ? Price * Quantity : 0;
            }
        }

        private void button1_Click(object sender, EventArgs e)
        {
            try
            {
                // Initialize menu items
                MenuItem coffeebest = new MenuItem
                {
                    Name = "coffeebest",
                    Price = coffee.Checked ? double.Parse(tbCoffeePrice.Text) : 0,
                    Quantity = coffee.Checked ? int.Parse(tbCoffeeQuantity.Text) : 0,
                    Selected = coffee.Checked
                };

                MenuItem greenTea = new MenuItem
                {
                    Name = "Green Tea",
                    Price = grr.Checked ? double.Parse(tbGreenCoffeePrice.Text) : 0,
                    Quantity = grr.Checked ? int.Parse(tbGreenCoffeeQuantity.Text) : 0,
                    Selected = grr.Checked
                };

                MenuItem noodlebest = new MenuItem
                {
                    Name = "Noodlebest",
                    Price = noodle.Checked ? double.Parse(tbNoodlePrice.Text) : 0,
                    Quantity = noodle.Checked ? int.Parse(tbNoodleQuantity.Text) : 0,
                    Selected = noodle.Checked
                };

                MenuItem pizzabest = new MenuItem
                {
                    Name = "Pizzabest",
                    Price = pizza.Checked ? double.Parse(tbPizzaPrice.Text) : 0,
                    Quantity = pizza.Checked ? int.Parse(tbPizzaQuantity.Text) : 0,
                    Selected = pizza.Checked
                };

                // Calculate totals
                double totalFood = noodlebest.TotalPrice() + pizzabest.TotalPrice();
                double totalDrink = coffeebest.TotalPrice() + greenTea.TotalPrice();
                double grandTotal = totalFood + totalDrink;

                // Apply discounts
                if (all.Checked)
                {
                    double discount = double.Parse(tbDiscountAll.Text);
                    grandTotal = ApplyDiscount(grandTotal, discount);
                }
                else if (rbDiscountBeverage.Checked)
                {
                    double discount = double.Parse(tbDiscountBeverage.Text);
                    totalDrink = ApplyDiscount(totalDrink, discount);
                    grandTotal = totalFood + totalDrink;
                }
                else if (rbDiscountFood.Checked)
                {
                    double discount = double.Parse(tbDiscountFood.Text);
                    totalFood = ApplyDiscount(totalFood, discount);
                    grandTotal = totalFood + totalDrink;
                }

                // Display total
                tbTotal.Text = grandTotal.ToString("F2");

                // Calculate change
                double cashReceived = double.Parse(tbCash.Text);
                if (cashReceived >= grandTotal)
                {
                    double change = cashReceived - grandTotal;
                    tbChange.Text = change.ToString("F2");

                    // Calculate denominations for change
                    CalculateDenominations(change);
                }
                else
                {
                    MessageBox.Show("เงินที่ได้รับไม่เพียงพอ");
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("เกิดข้อผิดพลาด: " + ex.Message);
            }
        }

        // Function to apply discount
        private double ApplyDiscount(double total, double discountPercentage)
        {
            return total - (total * (discountPercentage / 100));
        }

        // Function to calculate denominations
        private void CalculateDenominations(double change)
        {
            int[] denominations = { 1000, 500, 100, 50, 20, 10, 5, 1 };
            int remainingChange = (int)Math.Round(change);

            foreach (int denom in denominations)
            {
                int count = remainingChange / denom;
                remainingChange %= denom;

                switch (denom)
                {
                    case 1000:
                        tb1000.Text = count.ToString();
                        break;
                    case 500:
                        tb500.Text = count.ToString();
                        break;
                    case 100:
                        tb100.Text = count.ToString();
                        break;
                    case 50:
                        tb50.Text = count.ToString();
                        break;
                    case 20:
                        tb20.Text = count.ToString();
                        break;
                    case 10:
                        tb10.Text = count.ToString();
                        break;
                    case 5:
                        tb5.Text = count.ToString();
                        break;
                    case 1:
                        tb1.Text = count.ToString();
                        break;
                }
            }
        }
    }
}
