import mysql.connector

def fetch_data_from_db():
    try:
        # Connect to the MySQL database
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="",
            database="color_db"
        )
        cursor = connection.cursor()

        # Fetch data from the table
        cursor.execute("SELECT color, frequency FROM shirt_colors")
        results = cursor.fetchall()

        # Generate HTML content
        html_content = """
        <html>
        <head>
            <title>Shirt Colors Data</title>
            <style>
                body {
                    font-family: Arial, sans-serif;
                    margin: 20px;
                }
                table {
                    width: 100%;
                    border-collapse: collapse;
                    margin-top: 20px;
                }
                th, td {
                    border: 1px solid #ddd;
                    padding: 8px;
                    text-align: left;
                }
                th {
                    background-color: #f4f4f4;
                }
            </style>
        </head>
        <body>
            <h1>Shirt Colors Data</h1>
            <table>
                <thead>
                    <tr>
                        <th>Color</th>
                        <th>Frequency</th>
                    </tr>
                </thead>
                <tbody>
        """

        # Populate the table rows
        for color, frequency in results:
            html_content += f"<tr><td>{color}</td><td>{frequency}</td></tr>"

        # Close the table and HTML structure
        html_content += """
                </tbody>
            </table>
        </body>
        </html>
        """

        return html_content

    except mysql.connector.Error as e:
        print(f"Error: {e}")
        return None
    finally:
        if connection:
            cursor.close()
            connection.close()

# Save the generated HTML to a file
html_output = fetch_data_from_db()
if html_output:
    with open("shirt_colors.html", "w") as file:
        file.write(html_output)
        import webbrowser

# Open the generated HTML file in the default web browser
        webbrowser.open("shirt_colors.html")

        print("HTML file generated: shirt_colors.html")
