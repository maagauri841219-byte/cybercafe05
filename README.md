<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cyber Cafe - Payment Gateway</title>
    <script src="https://checkout.razorpay.com/v1/checkout.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
            color: #fff;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            margin-top: 20px;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            color: #00ffcc;
            text-shadow: 0 0 10px rgba(0, 255, 204, 0.5);
        }
        
        p {
            font-size: 1.1rem;
            line-height: 1.6;
            margin-bottom: 20px;
        }
        
        .payment-options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .payment-card {
            background: rgba(255, 255, 255, 0.15);
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            cursor: pointer;
        }
        
        .payment-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            background: rgba(255, 255, 255, 0.2);
        }
        
        .payment-card h3 {
            margin-bottom: 15px;
            color: #00ffcc;
        }
        
        .payment-card .amount {
            font-size: 1.8rem;
            font-weight: bold;
            margin: 10px 0;
            color: #fff;
        }
        
        .payment-card .time {
            font-size: 0.9rem;
            color: #ccc;
        }
        
        .btn {
            background: linear-gradient(45deg, #00ffcc, #00ccff);
            color: #000;
            border: none;
            padding: 12px 25px;
            border-radius: 50px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 10px;
            width: 100%;
        }
        
        .btn:hover {
            background: linear-gradient(45deg, #00ccff, #00ffcc);
            box-shadow: 0 0 15px rgba(0, 255, 204, 0.5);
            transform: scale(1.05);
        }
        
        .custom-amount {
            margin-top: 30px;
            text-align: center;
        }
        
        .custom-amount input {
            width: 100%;
            padding: 12px;
            border-radius: 50px;
            border: 2px solid #00ffcc;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            font-size: 1rem;
            text-align: center;
            margin-bottom: 15px;
            outline: none;
        }
        
        .custom-amount input::placeholder {
            color: #ccc;
        }
        
        .custom-amount input:focus {
            background: rgba(255, 255, 255, 0.2);
            box-shadow: 0 0 10px rgba(0, 255, 204, 0.5);
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            color: #ccc;
            font-size: 0.9rem;
        }
        
        .logo {
            font-size: 2rem;
            font-weight: bold;
            color: #00ffcc;
            margin-bottom: 20px;
            text-align: center;
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .payment-options {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="logo">CYBER CAFE 05</div>
    
    <div class="container">
        <header>
            <h1>Secure Payment Gateway</h1>
            <p>Select a package or enter a custom amount to proceed with payment. All transactions are secure and encrypted.</p>
        </header>
        
        <div class="payment-options">
            <div class="payment-card">
                <h3>Basic Package</h3>
                <p>1 hour of internet access</p>
                <div class="amount">₹50</div>
                <button class="btn" onclick="pay(50)">Pay Now</button>
            </div>
            
            <div class="payment-card">
                <h3>Standard Package</h3>
                <p>3 hours of internet access</p>
                <div class="amount">₹120</div>
                <button class="btn" onclick="pay(120)">Pay Now</button>
            </div>
            
            <div class="payment-card">
                <h3>Premium Package</h3>
                <p>Full day access</p>
                <div class="amount">₹300</div>
                <button class="btn" onclick="pay(300)">Pay Now</button>
            </div>
        </div>
        
        <div class="custom-amount">
            <h3>Custom Amount</h3>
            <input type="number" id="customAmount" placeholder="Enter amount in ₹" min="10">
            <button class="btn" onclick="payCustom()">Pay Custom Amount</button>
        </div>
    </div>
    
    <footer>
        <p>© 2023 Cyber Cafe 05. All rights reserved.</p>
        <p>Secure payments powered by Razorpay</p>
    </footer>

    <script>
        // Your Razorpay API Key
        const RAZORPAY_KEY_ID = 'rzp_live_RH7DPL7LtFs1WG';
        
        function pay(amount) {
            // Convert amount to paise (Razorpay expects amount in the smallest currency unit)
            const amountInPaise = amount * 100;
            
            const options = {
                key: RAZORPAY_KEY_ID,
                amount: amountInPaise,
                currency: 'INR',
                name: 'Cyber Cafe 05',
                description: 'Internet Access Payment',
                image: 'https://maagauri841219-byte.github.io/cybercafe05/',
                handler: function(response) {
                    alert('Payment successful! Payment ID: ' + response.razorpay_payment_id);
                    // Here you would typically send this payment ID to your server for verification
                },
                prefill: {
                    name: 'Customer',
                    email: 'customer@example.com',
                    contact: '+919876543210'
                },
                notes: {
                    address: 'Cyber Cafe 05'
                },
                theme: {
                    color: '#00ffcc'
                }
            };
            
            const rzp = new Razorpay(options);
            rzp.open();
        }
        
        function payCustom() {
            const customAmount = document.getElementById('customAmount').value;
            if (!customAmount || customAmount < 10) {
                alert('Please enter a valid amount (minimum ₹10)');
                return;
            }
            
            pay(parseInt(customAmount));
        }
    </script>
</body>
</html>
