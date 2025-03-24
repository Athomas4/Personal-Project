
//Hit F6 to Test

//M4T2
//Andrae Thomas
//3-19-2025

#include "raylib.h"
#include <stdlib.h>

//------------------------------------------------------------------------------------
// Program main entry point
//------------------------------------------------------------------------------------
int main(void)
{
    // Initialization
    //--------------------------------------------------------------------------------------
    const int screenWidth = 1024;
    const int screenHeight = 768;
    
    int tile_width = 80;
    int tile_length = 60;
    int tile_height = 40;
    Color color1 = BLUE;
    Color color2 = BLUE;
    int c_x = 0; 
    int c_y = 0;
    
    // Box to Collect
    int collect_block_width = 20;
    int collect_block_height = 20;
    Vector2 collect_block_position = { (float)(rand() % (screenWidth - collect_block_width)),
                                       (float)(rand() % (screenHeight - collect_block_height)) };
    bool collected = false;
    int points = 0;

    InitWindow(screenWidth, screenHeight, "raylib [core] example - basic window");
    
    // Helper Properties
    typedef struct Helper {
        Vector2 position;
        bool active;
    } Helper;
    
    Helper helpers[10]; // Array to store multiple helpers
    int helperCount = 0;
    // Cost of a Helper
    int helperCost = 5;
    // Building properties
    int building_width = 50;
    int building_height = 50;
    Vector2 building_position = { 10, screenHeight - building_height - 10 };

    SetWindowState(FLAG_BORDERLESS_WINDOWED_MODE);
    SetTargetFPS(60);  // Set our game to run at 60 frames-per-second
    //--------------------------------------------------------------------------------------

    // Color for helpers
    Color helperColor = ORANGE;
    
    // Menu dimensions and position
    int menu_width = 250;
    int menu_height = 100;
    int menu_x = screenWidth - menu_width - 10;
    int menu_y = screenHeight - menu_height - 10;

    // Main game loop
    while (!WindowShouldClose())  // Detect window close button or ESC key
    {
        // Update logic
        //----------------------------------------------------------------------------------
        if (IsKeyDown(KEY_RIGHT)) c_x += 5; // Move right
        if (IsKeyDown(KEY_LEFT)) c_x -= 5; // Move Left
        if (IsKeyDown(KEY_UP)) c_y -= 5; // Move up
        if (IsKeyDown(KEY_DOWN)) c_y += 5; // Move down
        
        // Ensure rectangle stays within boundary
        if (c_x < 0) c_x = 0;
        if (c_y < 0) c_y = 0;
        if (c_x > screenWidth - tile_width) c_x = screenWidth - tile_width;
        if (c_y > screenHeight - tile_height) c_y = screenHeight - tile_height;
        
        Rectangle tile = { c_x, c_y, tile_width, tile_height };
        Rectangle collect_block = { collect_block_position.x, collect_block_position.y, collect_block_width, collect_block_height };
        
        if (CheckCollisionRecs(tile, collect_block)) {
            // If collided, set collected to true and respawn the block
            collected = true;
            points += 5;
            collect_block_position.x = (float)(rand() % (screenWidth - collect_block_width));
            collect_block_position.y = (float)(rand() % (screenHeight - collect_block_height));
        }

        // Menu for purchasing helpers
        if (CheckCollisionPointRec(GetMousePosition(), (Rectangle){ menu_x + 10, menu_y + 10, 180, 40 }) && IsMouseButtonPressed(MOUSE_BUTTON_LEFT)) {
            if (points >= helperCost && helperCount < 10) {  // Check if player has enough points
                points -= helperCost;
                helpers[helperCount].position = building_position;  // Set the helper's starting position
                helpers[helperCount].active = true;  // Activate the helper
                helperCount++;
            }
        }

        // Make helpers go collect boxes
        for (int i = 0; i < helperCount; i++) {
            if (helpers[i].active) {
                // Move helper towards the collect block
                if (collect_block_position.x > helpers[i].position.x)
                    helpers[i].position.x += 1;
                if (collect_block_position.x < helpers[i].position.x)
                    helpers[i].position.x -= 1;
                if (collect_block_position.y > helpers[i].position.y)
                    helpers[i].position.y += 1;
                if (collect_block_position.y < helpers[i].position.y)
                    helpers[i].position.y -= 1;

                // Check for helper block collection
                if (CheckCollisionRecs((Rectangle){ helpers[i].position.x, helpers[i].position.y, 20, 20 }, collect_block)) {
                    points += 5;
                    collect_block_position.x = (float)(rand() % (screenWidth - collect_block_width));
                    collect_block_position.y = (float)(rand() % (screenHeight - collect_block_height));
                }
            }
        }

        // Draw the game screen
        //----------------------------------------------------------------------------------
        BeginDrawing();

        ClearBackground(BLACK);

        // Draw player tile
        DrawRectangle(c_x, c_y, tile_width, tile_height, color1);

        // Draw collected block
        DrawRectangle((int)collect_block_position.x, (int)collect_block_position.y, collect_block_width, collect_block_height, RAYWHITE);

        // Draw Helper building
        DrawRectangle((int)building_position.x, (int)building_position.y, building_width, building_height, DARKGRAY);
        DrawText("Base", (int)building_position.x + 5, (int)building_position.y + 20, 10, WHITE);

        // Draw the menu background
        DrawRectangle(menu_x, menu_y, menu_width, menu_height, DARKGRAY);
        
        // Display the "Buy Helpers" button
        DrawRectangle(menu_x + 10, menu_y + 10, 180, 40, BLUE);
        DrawText("Buy Helper", menu_x + 20, menu_y + 20, 20, WHITE);
        
        // Display the helper cost and helper count
        DrawText(TextFormat("Cost: %d", helperCost), menu_x + 10, menu_y + 50, 15, WHITE);
        DrawText(TextFormat("Helpers: %d", helperCount), menu_x + 10, menu_y + 70, 15, WHITE);

        // Draw helper blocks
        for (int i = 0; i < helperCount; i++) {
            if (helpers[i].active) {
                DrawRectangle((int)helpers[i].position.x, (int)helpers[i].position.y, 20, 20, helperColor);
            }
        }

        // Draw points
        DrawText(TextFormat("Points: %d", points), 10, 10, 20, WHITE);

        EndDrawing();
        //----------------------------------------------------------------------------------
    }

    // De-Initialization
    //--------------------------------------------------------------------------------------
    CloseWindow();  // Close window and OpenGL context
    //--------------------------------------------------------------------------------------
//Getting "cc1.exe: fatal error: Game.c: No such file or directory"
    return 0;
}
